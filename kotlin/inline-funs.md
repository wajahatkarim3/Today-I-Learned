# Inline Functions in Kotlin

While looking through answers on _StackOverflow_ or codes through _GitHub_ about Kotlin, you might have noticed keywords like ```inline```, ```noinline``` or ```crossinline``` in the methods signatures. Today, I learned about what ```inline``` is really about. And will explore other types later.

### What is inline method, then?

Say you have a higher order function in Kotlin,

```kotlin
fun higherOrderFunction(callback: () -> Unit) {
        
        doSomething()
        callback()
        doAnotherThing()
        
}
```

Now, when this is converted to **Java**, this will look something like this:

```java
public void higherOrderFunction(Function callback) {
 
    doSomething();
    callback.invoke();       //invoke will execute the logic in your lambda.
    doAnotherThing();

}
```

Note the ```invoke()``` call above. For example, if we call our ```higherOrderFunction()```, we can get something like this:

```kotlin
    // My regular code
    // Other logics
    
    higherOrderFunction( {
        // My callback logic
        doAnyMethodCall()
    })
```

When converted in Java, this will become something like this:

```java
    // My regular code
    // Other logics
    
    higherOrderFunction( new Function() {
        // My callback logic
        doAnyMethodCall()
    });
```

Now, if we call this method in any loop, this can allocate quite a chunk of the memory and can cause performance issues a lot. That's where ```inline``` comes in. 

Adding the ```inline``` keyword in your higher order function will prevent such. You’ll end up having your ```callback``` method’s code and the higher order function’s code inlined at the calling site. The method will be like this:

```kotlin
// Please note the inline keyword before method signature
inline fun higherOrderFunction(callback: () -> Unit) {
        
        doSomething()
        callback()
        doAnotherThing()
}
```

Now, when we call this method as we do:

```kotlin
    // My regular code
    // Other logics
    
    higherOrderFunction( {
        // My callback logic
        doAnyMethodCall()
    })
```

Then instead of creating ananymous methods, compiler will copy all the ```callback``` method lines in the ```higherOrderFunction()``` like this:

```java
//note that the entire `higherOrderFunction` method
//was copied into this method during the inlining.

    // My regular code
    // Other logics
  
    doSomething()
    doAnyMethodCall()           // This line has been copied from callback method's body
    doAnotherThing()

```

Now, you’ll be tempted to add the ```inline``` keyword to all your higher order functions. With ```inline``` functions, you will not be able to access private members/methods of your enclosing class. You will need to make those members/methods ```internal``` and then annotate them with ```@PublishedApi```.
