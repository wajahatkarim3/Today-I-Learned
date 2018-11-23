# Multiple Runtime Permissions in Android Without Any Third-Party Libraries

In my current project at work, I had a task to add runtime permissions in an android app whose code is very old and using legacy methods and frameworks/tools. Normally, I use [Ted Permissions](https://github.com/ParkSangGwon/TedPermission) in all my apps for the runtime permissions and I must say that it's one hell of an amazing library I ever saw and given the complex scenerio and flow of runtime permissions in Android (thanks to Google who always makes sure to make every thing more complicated than ever), this library makes the runtime permissions like a breeze. Wonderful job [*ParkSangGwon*](https://github.com/ParkSangGwon/).

Android introduced [Runtime Permissions in API Level 23 (Android Marshmallow 6.0) or later versions](https://developer.android.com/guide/topics/permissions/overview). The permissions has a very complicated flow to implement as shown in the below image:
![](https://raw.githubusercontent.com/ParkSangGwon/TedPermission/master/Screenshot_cases.png)

Today, when I was looking for very simple approach to do this, I was shocked to see that how Google has made this simple thing so much complicated and confusing to implement just like how they have done with Camera APIs. It took me a whole day just to add two simple runtime permissions at the start of my code. Here's how I have done it.

## 1. Add permissions in Android Manifest file
My app needed *Storage* and *Location* permissions. So, add these permissions in the Android Manifest file like this:
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android">
    
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" /> <!-- dangerous permission -->
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" /> <!-- dangerous permission -->
    
    <application>
       
    </application>
  
</manifest>
```
