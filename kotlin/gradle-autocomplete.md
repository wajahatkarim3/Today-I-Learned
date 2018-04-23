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
}

object Dependencies {
    val kotlin_stdlib = "org.jetbrains.kotlin:kotlin-stdlib:${Versions.kotlin}"
}
```
You will be able to add this dependency straight away (with the help of autocomplete & navigation) across all of your modules with:
```groovy
implementation Dependencies.kotlin_stdlib
```

Credits:
https://caster.io/lessons/gradle-dependency-management-using-kotlin-and-buildsrc-for-buildgradle-autocomplete-in-android-studio.
