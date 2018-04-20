# Safely accessing lateinit properties in Kotlin

Kotlin, by design, doesn't allow a non-null variable to be left uninitialized during it's declaration. 
Whenever you declare a lateinit var, you need to initialize it before you access it. Otherwise, you'll be greeted with a fancy exception like this:
```
Exception in thread "main" kotlin.UninitializedPropertyAccessException: lateinit property fullName has not been initialized
	at UninitializedPropertyKt.main(UninitializedProperty.kt:3)
```
Don't mistake this for a puny exception. It'll crash your app. 
So, how to solve this problem?

## Taking the rookie approach

The most lucrative solution to this problem would be to make the property a regular nullable one instead of a lateinit var and assign a value later on.
You can do something like this:
```kotlin
var fullName: String? = null
```
And then just do a plain null check or kotlin null-check operator ```?``` whenever you're accessing the value.
```kotlin
if (fullName != null) {
    print("Hi, $fullName")
}
var length = fullName?.length ?: 0
```
Kind of like Java. But hang on a sec, Kotlin is supposed to be better than Java. Also, one of the USPs of Kotlin was eliminating the fiasco caused by a ```NullPointerException```.

So, why go the traditional route?

Here's a better solution.

## Going the Kotlinish way
If you're using Kotlin 1.2, you can easily check whether a lateinit variable has been initialized or not. If not, well, you can always use the not null approach.

Anyways, here's how you can check if a lateinit var has been initialized or not:
```kotlin
if (::fullName.isInitialized) {
    print("Hi, $fullName")
}
```
