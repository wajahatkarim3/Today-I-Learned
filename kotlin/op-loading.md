# Operator Overloading in Kotlin

Kotlin supports a technique called **conventions**, everyone should be familiar with. For example, if you define a special method `plus` in your class, you can use the `+` operator *by convention*: That's called Kotlin Operator Overloading.

In this article, I want to show you which conventions you can use and I will also provide a few [Kotlin](http://kotlinlang.org) code examples that demonstrate the concepts. Kotlin defines conventions that we can apply by implementing methods that comply with predefined names like `plus`. This is contrary to how *Java* does provide equivalent language features, which is by specific classes like `Iterable` to make objects usable in `for` loops for example.

In the following descriptions, I’m going to use a class representing a *mathematical fraction*. This class will *not be*
 complete and also won’t provide mathematically perfect implementations.
 Just in case, you feel like knowing a much better implementation.

