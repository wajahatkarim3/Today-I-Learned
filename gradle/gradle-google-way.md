# Gradle Dependencies using Google's Way

This is Google’s recommended way of doing this as seen in the Android documentation.  It is also used in lots of Android projects, like ButterKnife and Picasso.

This method is great for upgrading libraries like the support library.  Every support library dependency has the same version number, so only having to change this in one place is 💯.  The same things goes for Retrofit, and many other libraries.

## Root-level build.gradle
```groovy
ext {
  versions = [
    support_lib: "27.0.2",
    retrofit: "2.3.0",
    rxjava: "2.1.9"
  ]
  libs = [
    support_annotations: "com.android.support:support-annotations:${versions.support_lib}",
    support_appcompat_v7: "com.android.support:appcompat-v7:${versions.support_lib}",
    retrofit :"com.squareup.retrofit2:retrofit:${versions.retrofit}",
    retrofit_rxjava_adapter: "com.squareup.retrofit2:adapter-rxjava2:${versions.retrofit}",
    rxjava: "io.reactivex.rxjava2:rxjava:${versions.rxjava}"
  ]
}
```

## App or Module's build.gradle
```groovy
implementation libs.support_annotations
implementation libs.support_appcompat_v7
implementation libs.retrofit
implementation libs.retrofit_rxjava_adapter
implementation libs.rxjava
```
