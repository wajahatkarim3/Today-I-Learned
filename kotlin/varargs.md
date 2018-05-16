# Variable Number of Arguments in Methods in Kotlin

Often times, there are situation where we don't know much parameters or arguments would be passed in method. This is done in Java using three dots like this:

```java
void anySampleMethod(Int... intList)        // Note the three dots after the type
{
    // Now you can access these in for loop.
    for (Int myInt : intList)
    {
        // Do anything with myInt here
        myInt += 10;
    }
}

// When calling this method
anySampleMethod(1,2,3,4);       // Passed 4 arguments
anySampleMethod(2);             // Passed 1 argument
```

To do this in Kotlin, we will use ```vararg``` which means variable arguments. 

```kotlin
fun anySampleMethod(varargs intList: Int)       // Note the vararg before the variable name and the type
{
    // Now you can access these arguments in loop
    for (myInt in intList)
    {
        // Do anything with myInt here
        myInt += 10
    }
}


// When calling this method
anySampleMethod(1,2,3,4);       // Passed 4 arguments
anySampleMethod(2);             // Passed 1 argument
```
