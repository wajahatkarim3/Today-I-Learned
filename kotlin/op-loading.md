# Operator Overloading in Kotlin

Kotlin supports a technique called **conventions**, everyone should be familiar with. For example, if you define a special method `plus` in your class, you can use the `+` operator *by convention*: That's called Kotlin Operator Overloading.

In this article, I want to show you which conventions you can use and I will also provide a few [Kotlin](http://kotlinlang.org) code examples that demonstrate the concepts. Kotlin defines conventions that we can apply by implementing methods that comply with predefined names like `plus`. This is contrary to how *Java* does provide equivalent language features, which is by specific classes like `Iterable` to make objects usable in `for` loops for example.

In the following descriptions, I’m going to use a class representing a *mathematical fraction*. This class will *not be*
 complete and also won’t provide mathematically perfect implementations.
 Just in case, you feel like knowing a much better implementation.

```kotlin
data class Fraction(val numerator: Int, val denominator: Int) {
    val decimal by lazy { numerator.toDouble() / denominator }

    override fun toString() = "$numerator/$denominator"
}
```

We have a data class with two user-defined properties: *numerator* and *denominator*. If we print a `Fraction` to the console, it’s supposed to look like “2/3”, so the `toString()` is being overridden. Also, for comparing two `Fraction` instances, a [lazy](https://kotlinlang.org/docs/reference/delegated-properties.html) property is included for providing the decimal value of the fraction.

#### Arithmetic Operators

Overloading operators enables us to use `+` in other classes than `Int` or `String`, you can use Kotlin’s predefined naming conventions to provide this functionality in any class. Let’s add it to our `Fraction` and see how it’s done.

```kotlin
data class Fraction(val numerator: Int, val denominator: Int) {
    //...

    operator fun plus(add: Fraction) =
            if (this.denominator == add.denominator){
                Fraction(this.numerator + add.numerator, denominator)
            } else {
                val a = this * add.denominator //(1)
                val b = add * this.denominator
                Fraction(a.numerator + b.numerator, a.denominator)
            }

    operator fun times(num: Int) = Fraction(numerator * num, denominator * num)

}
```

As you can see here, the `plus` method is defined as an **operator function** by using the `operator` keyword, which is relevant because otherwise, the compiler wouldn’t treat `plus` as a special function complying with a convention. As a second binary operator, `times` is implemented and also used in the implementation of `plus`, as you can see in **(1)**. The statement `this * add.denominator` is translated to `this.times(add.denominator)` by the compiler.
 Let’s see how the `Fraction` can be used outside of this class now.

```kotlin
var sum = Fraction(2, 3) + Fraction(3, 2)
println("Sum: ${sum.decimal}")
```

As expected, adding two fractions with `+` is now possible, as well as using `*` would be as we also provided a `times` function. If we wanted to be able to multiply a `Fraction` with another `Fraction`, we’d have to overload the `times` function to accept a `Fraction` parameter instead of an `Int`. *Thus, it isn’t a problem to provide multiple operator functions with different parameter types*.

Besides *binary* operators, we can also overload *unary* operators to do something like negating a fraction: `-Fraction(2,3)` or incrementing/decrementing by using `--`/`++`. Please have a look at the [documentation](https://kotlinlang.org/docs/reference/operator-overloading.html) to learn how the naming conventions look like for these cases.