# Domain-Specific Language (DSL) using Kotlin

The concept of DSL or [domain-specific language](https://en.wikipedia.org/wiki/Domain-specific_language) is pretty simple, it gives a context of what we doing, for example, a common Android Gradle script will have the following structure:

```groovy
android {
  compileSdkVersion 26
  defaultConfig {
    applicationId "com.example.you"
    minSdkVersion 15
    targetSdkVersion 26
    versionCode 1
    versionName "1.0
  }
}
```

Inside the Android section, we put android specific configuration and inside defaultConfig we put the general configuration, by this structure we know the context of each section.

The same idea of structure the use of API in that way can be applied also on single class with specific responsibility, for example, if we want to write an article builder class it will be nice if we could use this class in the following way:

```kotlin
val articleBuilder = ArticleBuilder()
articleBuilder {
  title = "This is the title"
  addParagraph {
    body = "This is the first paragraph body"
  }
  addParagraph {
    body = "This is the first paragraph body"
    imageUrl = "https://path/to/url"
  }
}
```

This is a very declarative way to use this article build, it’s almost describing the way we build the article in natural language. In kotlin it’s very easy to build such small DSL since the language provides us a lot of tools. The first step will be adding ```invoke``` operator to the class which gets as parameter lambda with receiver:

```kotlin
class ArticleBuilder {
  lateinit var title: String
  operator fun invoke(block: ArticleBuilder.() -> Unit) {
     block()
  }
}
```

Next step will be to enable to add some paragraph, in order to use it we will first add ```Paragraph``` class with 2 properties, ```body``` and ```imageUrl```:

```kotlin
class Paragraph {
  lateinit var body: String
  var imageUrl: String? = null
}
```

Now we need to add a paragraph and this will be done by adding a new method with the name ```addParagraph``` which gets as a parameter a lambda with ```Paragraph``` receiver, this will allowed us to set the paragraph fields:

```kotlin
fun addParagraph(block: Paragraph.() -> Unit) {
  val paragraph = Paragraph()
  paragraph.block()
  paragraphs.add(paragraph)
}
```

So our final code and use of the code is:

```kotlin
class Paragraph {
  lateinit var body: String
  var imageUrl: String? = null
}

class ArticleBuilder {
  lateinit var title: String
  val paragraphs = mutableListOf<Paragraph>()

  operator fun invoke(block: ArticleBuilder.() -> Unit) {
    block()
  }

  fun addParagraph(block: Paragraph.() -> Unit) {
    val paragraph = Paragraph()
    paragraph.block()
    paragraphs.add(paragraph)
  }
}
```
