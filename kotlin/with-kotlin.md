# The with() operator in Kotlin

There are times when there's multiple lines of operations on the same object, and we do it by calling ```myObject``` instance everytime we do any operation. This makes our code repetitive and makes it look bad and ugly. 

For example, when we put different values in the ```Intent```, we do it like this:

```kotlin
    var intent = Intent()
    intent.putExtra("myInt", 0)
    intent.putExtra("myBool", false)
    intent.putExtra("myString", "hello string")
    startActivity(intent)
```

And when we receive this ```Intent``` in any other ```Activity```, then we get all the values like this:

```kotlin
    var intent = getMyIntent()
    var myInt = intent.getInt("myInt", 0)
    var myBool = intent.getBoolean("myBool", false)
    var myString = intent.getString("myString")
```

Again, same ugly repetitive code block. Kotlin provides a better solution to this problem by using ```with``` operator. We can write above codes like this now:

```kotlin
    var intent = Intent()
    with(intent)
    {
        putExtra("myInt", 0)
        putExtra("myBool", false)
        putExtra("myString", "hello string")
    }
    startActivity(intent)
```

See! How clean this is. Similarly, we can use it at receiving ```Intent``` as well:

```kotlin
    var intent = getMyIntent()
    with(intent)
    {
        var myInt = getInt("myInt", 0)
        var myBool = getBoolean("myBool", false)
        var myString = getString("myString")
    }
```
