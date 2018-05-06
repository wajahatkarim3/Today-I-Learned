# Creating Separate Modules for Debug And Release

When it comes to debugging tools and libraries for Android such as [Stetho](http://facebook.github.io/stetho/) or [Chuck](https://github.com/jgilfelt/chuck) or the Jake Wharton's great [Leak Canary](https://github.com/square/leakcanary), there're almost separate modules for ```debug``` and ```release``` builds. For example, when you add _Leak Canary_ in your app, you will be adding dependencies like this:

```groovy
  debugImplementation 'com.squareup.leakcanary:leakcanary-android:1.5.4'
  releaseImplementation 'com.squareup.leakcanary:leakcanary-android-no-op:1.5.4'
```

Please note that there are two separate modules or AARs for ```debug``` and ```release```. Its a common practice to call ```release``` variants for debugging and inspection tools as *NO OP* or *No Operations*. The purpose of this ```no-op``` modules is to do absolutely nothing in the code. Its just an empty wrapper of the ```debug``` module to avoid the compile time errors such as ```Unresolved method name``` or so. 

Few days ago, I was working on a debugging tool plugin for [Hyperion](https://github.com/willowtreeapps/Hyperion-Android) called as [DBFlow Manager Hyperion](https://github.com/wajahatkarim3/DBFlowManager-Hyperion-Plugin) plugin. I didn't know about ```no-op``` yet and I simply created a module which did all the work for the DBFlow databases inspection and played very well with Hyperion. I added it in the app using ```debugImplementation``` and everything was great until I made a ```release``` APK of the app.

I was unable to compile a ```release``` APK now. The error I was getting was ```Unresolved object: DBFlowManager_Hyperion_Constants```. You can follow the whole issue (fixed and closed) at [here](https://github.com/wajahatkarim3/DBFlowManager-Hyperion-Plugin/issues/4). Apparently, the app was using ```DBFlowManager_Hyperion_Constants``` in the code and this class was only available for ```debug``` builds as we only included the plugin for debugging. But, since code was using a reference, so ```release``` APK couldn't find the class reference and it failed.

After a simple google search, I managed to learn about how it all works. I created another ```no-op``` module. All you have to do is add empty classes and methods and simply compile it. Now when ```release``` apk will be compiled, it will find all the references but code will do nothing at all. It will be just for a placeholder purpose.

For example, the ```DBFlowManagerPlugin``` class in the ```debug``` module looks like this:

```java
public class DBFlowManagerPlugin extends Plugin {

    @Nullable
    @Override
    public PluginModule createPluginModule() {
        return new DBFlowManagerModule();               // This is the functionality class
    }
}
```

While this same class in ```release``` and ```no-op``` module is empty and looks like this:

```java
public class DBFlowManagerPlugin {

    //@Override
    public Object createPluginModule() {
        return null;                                   // This is returning null. 
    }
}
```

Note that only class and method names are same. Even method's return type has been changed to ```Object```. It's all an empty and nothing-to-do module.

### References
https://medium.com/@orhanobut/no-op-versions-for-dev-tools-b0a865934398
https://xrubio.com/2016/10/disabling-removing-code-on-release-builds/
