# Go to Code Line from Logcat Output Line

Let's start by looking at our typical good'ol logcat window in Android Studio like this:

![](https://www.codeproject.com/KB/android/1078103/chpt03_007.png)

Now, when you get this kind of logs while coding your awesome apps, then your first question would be: **Where is the source code line for this log?**. So that you can know what problem is and how to fix it. But, Android Studio doesn't let you jump to the source code's exact line unless its a ```RuntimeException``` or simply called as ```Crash```. 

Now, some developers go to source code's exact line by navigation (```Ctrl + N```) and searching about ```tag``` etc. Or there might be other ways. Let's look at more cleaner and easier way to fix this issue.

### Traditional Way (using android.util.Log)
Add this class in your app code, and call it like ```LogUtil.prepand
