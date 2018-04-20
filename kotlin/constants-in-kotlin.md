# Where Should I Keep My Constants in Kotlin?
There has recently been a discussion online about the best approach for storing global constants, or ```public static final``` fields in Javaspeak. Here, we will discuss multiple methods for creating global constants in kotlin.

## Companion objects
There's no ```static``` keyword in Kotlin. If you want static access to certain fields or methods of your class, you should put them under a companion object. A naive way to declare a constant would look like this:
```kotlin
class Constants {  
  companion object {
    val FOO = "foo"
  }
}
```
The field will be available globally and accessible through 
```kotlin
var myval = Constants.FOO
```

## const vals
A simple way to improve our example is to mark ```FOO``` as ```const```:
```kotlin
class Constants {  
  companion object {
    const val FOO = "foo"
  }
}
```
But this will only with primitive types and Strings. This will not work for custom classes such as:
```kotlin
class Constants {  
  companion object {
    // won't compile
    const val FOO = Foo()
  }
}
```
A workaround is to use the ```@JvmField``` annotation on the ```val```:
```kotlin
class Constants {  
  companion object {
    @JvmField val FOO = Foo()
  }
}
```

## Dropping the class and the object
If all we need is just a set of constants - we can safely drop both the class and the object and use top-level vals:
```kotlin
const val FOO = "foo"  
```
For example, overall constants will be something like:
```kotlin
@file:JvmName("Constants")

package com.example.package.app

const val DB_NAME = "myofflinedb"
const val BASE_URL = "http://myexample.baseurl.com/"
const val DUMMY_VALUE = 14
```
Your file name can be anything but you can access the values by:
```kotlin
var mydb = Constants.DB_NAME
var myUrl = Constants.BASE_URL
```
