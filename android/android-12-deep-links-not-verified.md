# Android 12 Doesn't Open Deep Links 

I realized that in our current app, deep links were working in Android 11 or before but on Android 12. There have been some changes after Android 12, which can be [read in this blog post with all the checks and validations](https://zarah.dev/2022/02/08/android12-deeplinks.html). 

But even after adding `assetlinks.json` file and adding all the `intent-filter`s etc, Android 12 still wasn't detecting deep links. Turns out that `scheme`'s tag is need to be separated from the `data` tag like this:

Turn this 

```xml
<data android:scheme="http" android:host="www.example.com" />
```

into this one

```xml
<data android:scheme="http" />
<data android:host="www.example.com" />
```

In my specific app case, it was like this

// OLD

```xml
<data
  android:host="domain.name"
  android:pathPrefix="/videos"
  android:scheme="https" />
```

// NEW - Which is working on all including Android 12

```xml
<data android:scheme="https" />
<data
    android:host="domain.name"
    android:pathPrefix="/videos" />
```

**Note that there are two separate tags now**

Resources:
https://stackoverflow.com/a/68144368
https://zarah.dev/2022/02/08/android12-deeplinks.html
