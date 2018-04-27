# Go to Code Line from Logcat Output Line

Let's start by looking at our typical good'ol logcat window in Android Studio like this:

![](https://www.codeproject.com/KB/android/1078103/chpt03_007.png)

Now, when you get this kind of logs while coding your awesome apps, then your first question would be: **Where is the source code line for this log?**. So that you can know what problem is and how to fix it. But, Android Studio doesn't let you jump to the source code's exact line unless its a ```RuntimeException``` or simply called as ```Crash```. 

Now, some developers go to source code's exact line by navigation (```Ctrl + N```) and searching about ```tag``` etc. Or there might be other ways. Let's look at more cleaner and easier way to fix this issue.

After the fix, your logcat will be something similar to this.

![](https://cdn-images-1.medium.com/max/2000/1*pruJtyUU0U3-d6X1rdkuXQ.png)

## Traditional Way (using android.util.Log)
Add [this class](https://gist.github.com/wajahatkarim3/21fc299f22172c4a147ba3ce75d2d761#file-logutil-java) in your app code, and call it like ```LogUtil.d("MyTag", "My Custom Message")``` instead of ```Log.d("tag", "message")```.

```java
import java.util.regex.Matcher;
import java.util.regex.Pattern;
import android.util.Log;

public class LogUtil {

    private static final int CALL_STACK_INDEX = 1;
    private static final Pattern ANONYMOUS_CLASS = Pattern.compile("(\\$\\d+)+$");

    public static String prependCallLocation(String message) {
        // DO NOT switch this to Thread.getCurrentThread().getStackTrace(). The test will pass
        // because Robolectric runs them on the JVM but on Android the elements are different.
        StackTraceElement[] stackTrace = new Throwable().getStackTrace();
        if (stackTrace.length <= CALL_STACK_INDEX) {
            throw new IllegalStateException(
                    "Synthetic stacktrace didn't have enough elements: are you using proguard?");
        }
        String clazz = extractClassName(stackTrace[CALL_STACK_INDEX]);
        int lineNumber = stackTrace[CALL_STACK_INDEX].getLineNumber();
        message = ".(" + clazz + ".java:" + lineNumber + ") - " + message;
        return message;
    }

    /**
     * Extract the class name without any anonymous class suffixes (e.g., {@code Foo$1}
     * becomes {@code Foo}).
     */
    private static String extractClassName(StackTraceElement element) {
        String tag = element.getClassName();
        Matcher m = ANONYMOUS_CLASS.matcher(tag);
        if (m.find()) {
            tag = m.replaceAll("");
        }
        return tag.substring(tag.lastIndexOf('.') + 1);
    }
    
    public static void d(String tag, String message)
    {
        String newMessage = LogUtil.prependCallLocation(message);
        Log.d(tag, newMessage);
    }
    
    public static void w(String tag, String message)
    {
        String newMessage = LogUtil.prependCallLocation(message);
        Log.w(tag, newMessage);
    }
    
    public static void e(String tag, String message)
    {
        String newMessage = LogUtil.prependCallLocation(message);
        Log.e(tag, newMessage);
    }
    
    public static void v(String tag, String message)
    {
        String newMessage = LogUtil.prependCallLocation(message);
        Log.v(tag, newMessage);
    }
    
    public static void i(String tag, String message)
    {
        String newMessage = LogUtil.prependCallLocation(message);
        Log.i(tag, newMessage);
    }
    
    public static void wtf(String tag, String message)
    {
        String newMessage = LogUtil.prependCallLocation(message);
        Log.wtf(tag, newMessage);
    }   
}
```

## Timber Way
If you use [Timber](https://github.com/JakeWharton/timber), then you can use [this tree](https://gist.github.com/wajahatkarim3/21fc299f22172c4a147ba3ce75d2d761#file-logcodedebugtree-java) and attach it to the ```timber``` like a normal way. 

```java
import java.util.regex.Matcher;
import java.util.regex.Pattern;

import timber.log.Timber;

public class LinkingDebugTree extends Timber.DebugTree {

    private static final int CALL_STACK_INDEX = 4;
    private static final Pattern ANONYMOUS_CLASS = Pattern.compile("(\\$\\d+)+$");

    @Override
    protected void log(int priority, String tag, String message, Throwable t) {
        // DO NOT switch this to Thread.getCurrentThread().getStackTrace(). The test will pass
        // because Robolectric runs them on the JVM but on Android the elements are different.
        StackTraceElement[] stackTrace = new Throwable().getStackTrace();
        if (stackTrace.length <= CALL_STACK_INDEX) {
            throw new IllegalStateException(
                    "Synthetic stacktrace didn't have enough elements: are you using proguard?");
        }
        String clazz = extractClassName(stackTrace[CALL_STACK_INDEX]);
        int lineNumber = stackTrace[CALL_STACK_INDEX].getLineNumber();
        message = ".(" + clazz + ".java:" + lineNumber + ") - " + message;
        super.log(priority, tag, message, t);
    }

    /**
     * Extract the class name without any anonymous class suffixes (e.g., {@code Foo$1}
     * becomes {@code Foo}).
     */
    private String extractClassName(StackTraceElement element) {
        String tag = element.getClassName();
        Matcher m = ANONYMOUS_CLASS.matcher(tag);
        if (m.find()) {
            tag = m.replaceAll("");
        }
        return tag.substring(tag.lastIndexOf('.') + 1);
    }
}
```
