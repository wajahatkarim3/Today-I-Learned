# Generate Navigation Direction Classes in Kotlin with Safe Args Plugin

[Safe Args](https://developer.android.com/guide/navigation/navigation-pass-data) plugin allows you to pass data between destination fragments in Navigation Components in Android. This plugin generates some classes to make things easier.

## Java
Root `build.gradle`
```groovy
// Top-level build file where you can add configuration options common to all sub-projects/modules.

buildscript {
    repositories {
        google()
        maven { url "https://jitpack.io" }
        jcenter()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:3.6.0-alpha03'
        classpath "org.jetbrains.kotlin:kotlin-gradle-plugin:$kotlin_version"
        classpath "androidx.navigation:navigation-safe-args-gradle-plugin:$nav_version"
        // NOTE: Do not place your application dependencies here; they belong
        // in the individual module build.gradle files
    }
}

allprojects {
    repositories {
        google()
        maven { url "https://jitpack.io" }
        jcenter()
    }
}

task clean(type: Delete) {
    delete rootProject.buildDir
}

```

App `build.gradle`
```groovy
apply plugin: 'com.android.application'

apply plugin: 'kotlin-android'
apply plugin: 'kotlin-kapt'
apply plugin: 'kotlin-android-extensions'
apply plugin: 'androidx.navigation.safeargs'      // --> This Will Generate Java classes

android {
    // ...
}

dependencies {
    implementation fileTree(dir: 'libs', include: ['*.jar'])
    implementation "org.jetbrains.kotlin:kotlin-stdlib-jdk7:$kotlin_version"
    implementation 'androidx.appcompat:appcompat:1.0.2'
    implementation 'androidx.core:core-ktx:1.0.2'
    implementation 'androidx.constraintlayout:constraintlayout:2.0.0-beta1'

    // Navigation Architecture
    implementation "androidx.navigation:navigation-runtime-ktx:$nav_version"
    implementation "androidx.navigation:navigation-fragment-ktx:$nav_version"
    implementation "androidx.navigation:navigation-ui-ktx:$nav_version" // use -ktx for Kotlin
    
    // ...
}
kapt {
    generateStubs = true
}
androidExtensions {
    experimental = true
}
```

***

## Kotlin

Same Root `build.gradle`

App `build.gradle`
```groovy
apply plugin: 'com.android.application'

apply plugin: 'kotlin-android'
apply plugin: 'kotlin-kapt'
apply plugin: 'kotlin-android-extensions'
apply plugin: 'androidx.navigation.safeargs.kotlin'      // --> This Will Generate Kotlin classes. Please note .kotlin at the end.

android {
    // ...
}

dependencies {
    implementation fileTree(dir: 'libs', include: ['*.jar'])
    implementation "org.jetbrains.kotlin:kotlin-stdlib-jdk7:$kotlin_version"
    implementation 'androidx.appcompat:appcompat:1.0.2'
    implementation 'androidx.core:core-ktx:1.0.2'
    implementation 'androidx.constraintlayout:constraintlayout:2.0.0-beta1'

    // Navigation Architecture
    implementation "androidx.navigation:navigation-runtime-ktx:$nav_version"
    implementation "androidx.navigation:navigation-fragment-ktx:$nav_version"
    implementation "androidx.navigation:navigation-ui-ktx:$nav_version" // use -ktx for Kotlin
    
    // ...
}
kapt {
    generateStubs = true
}
androidExtensions {
    experimental = true
}
```
