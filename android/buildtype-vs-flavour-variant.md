# Difference between Build Type, Flavour, and Build Variant in Android

### Build Type
Build Type refers to build and packaging settings like signing configuration for a project. For example, ```debug``` and ```release``` build types. 
The ```debug``` will use android debug certificate for packaging the APK file. While, ```release``` build type will use user-defined release certificate for signing and packaging the APK.

Build types are defined in app's ```build.gradle``` file like this:
```groovy
    buildTypes {
        debug {
            applicationIdSuffix ".debug"
            debuggable true
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }
```

### Flavour
A flavor is used to specify custom features, minimum and target API levels, device and API requirements like layout, drawable and custom code (for example, if production code is slightly different than development code). This can help create different white label apps as well. Flavours can vary in adding different features or customizing exisitng features, different icons and resources, different styles and strings etc. A simple example of flavour can be to use the live server url as base url for Retrofit in one flavour while adding mock or testing server url for debugging purposes.

Flavours are defined in app's ```build.gradle``` file like this:
```groovy
  android {
    flavorDimensions "version"

    productFlavors {
        freeVersion {
            //select the dimension of flavor
            dimension "version"

            //configure applicationId for app published to Play store
            applicationId "com.pcc.flavors.free"

            //Configure this flavor specific app name published in Play Store
            resValue "string", "flavored_app_name", "Free Great App"

        }

        paidVersion {
            dimension "version"
            applicationId "com.pcc.flavors.paid"
            resValue "string", "flavored_app_name", "Paid Great App"
        }
    }
}
```

### Build Variant
The combination of Build Type and Flavor is known as Build Variant. For example, for above build types (debug and release) and product flavours (free and paid versions), build variants can be ```freeDebug```, ```freeRelease```, ```paidDebug```, ```paidRelease```.

You can view build variants in Android Studio at **View -> Tool Windows -> Build Variants**. It would be something like this:

![](https://1.bp.blogspot.com/--waVet_GCwY/Vm8WSV5w-JI/AAAAAAAACdc/HSapBccFfJ8/s640/image01.png)
