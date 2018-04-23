# Gradle Dependencies Management with Auto-Complete in Kotlin

![](https://handstandsam.com/wp-content/uploads/2015/02/AutoComplete.gif)

## How to do it

1. Create a root level directory called ```buildSrc```
2. Inside ```buildSrc``` create a file called ```build.gradle.kts``` and enable the kotlin-dsl plugin with the snippet below. Don’t forget to do a gradle sync to activate this plugin.
```groovy
import org.gradle.kotlin.dsl.`kotlin-dsl`

plugins {
    `kotlin-dsl`
}
```
3. Create directories ```src > main > java``` inside ```buildSrc``` .
4. Inside java, create a Kotlin file and give it any name you want (```Dependencies.kt``` for example).
5. Create as many object declarations as you want (ideally at least 1 for ```Versions``` and another for ```Dependencies```).

### Example
```kotlin
object Versions {
    val kotlin = "1.2.21"
    val support_lib = "27.0.2"
    val retrofit = "2.3.0"
    val rxjava = "2.1.9"
}

object Dependencies {
    val kotlin_stdlib = "org.jetbrains.kotlin:kotlin-stdlib:${Versions.kotlin}"
    val support_annotations = "com.android.support:support-annotations:${Versions.support_lib}"
    val support_appcompat_v7 = "com.android.support:appcompat-v7:${Versions.support_lib}"
    val retrofit = "com.squareup.retrofit2:retrofit:${Versions.retrofit}"
    val retrofit_rxjava_adapter = "com.squareup.retrofit2:adapter-rxjava2:${Versions.retrofit}"
    val rxjava = "io.reactivex.rxjava2:rxjava:${Versions.rxjava}"
}
```
You will be able to add this dependency straight away (with the help of autocomplete & navigation) across all of your modules with:
```groovy
implementation Dependencies.kotlin_stdlib
implementation Dependencies.support_annotations
implementation Dependencies.support_appcompat_v7
implementation Dependencies.retrofit
implementation Dependencies.retrofit_rxjava_adapter
implementation Dependencies.rxjava
```

Credits:
https://caster.io/lessons/gradle-dependency-management-using-kotlin-and-buildsrc-for-buildgradle-autocomplete-in-android-studio.
