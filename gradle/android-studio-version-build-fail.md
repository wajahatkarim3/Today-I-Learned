# This version of Android Studio cannot open this project, please retry with Android Studio x.x or newer

Today, I had to work on a few months old project. When I opened the project in Android Studio, the gradle sync failed with the following message.

```
This version of Android Studio cannot open this project, please retry with Android Studio x.x or newer"
```

I realized that the project was originally created in Android Studio Canary version 3.6 alpha/beta. But today I was using the Android Studio 3.5 stable release. That's why there was version mismatch error. At first, I tried to do the "Invalidate Cache and Restart". I thought this will fix the issue. But, it wasn't the issue of old gradle cache files. Rather it was about which gradle version project is using.

In my root project `build.gradle` file, I was using `3.6.0-alpha02` version.

```gradle
// Top-level build file where you can add configuration options common to all sub-projects/modules.

// .....
// .....

buildscript {
    ext.kotlin_version = '1.3.31'
    repositories {
        google()
        jcenter()
    }
    
    dependencies {
        classpath 'com.android.tools.build:gradle:3.6.0-alpha02'           <------ THIS LINE IS IMPORTANT
        
        classpath "org.jetbrains.kotlin:kotlin-gradle-plugin:$kotlin_version"
        // NOTE: Do not place your application dependencies here; they belong
        // in the individual module build.gradle files


    }
}

// .....
// .....

```

All I had to do was to change the version back where current Android Studio can supports it. So, for Android Studio 3.5, I will have to use 3.5.x version. I changed it to 3.5.0 stable release like this.

```gradle
  classpath 'com.android.tools.build:gradle:3.5.0'
```

And synced the project and voilaaaaa.... Done

### Reference
Some other suggestions to this problem are in this [StackOverflow Discussion](https://stackoverflow.com/questions/53331462/this-version-of-android-studio-cannot-open-this-project-please-retry-with-andro).
