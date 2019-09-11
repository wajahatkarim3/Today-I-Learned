# Forcing any dependency to be searched in any specific repository

Sometimes, we need to force any Android dependency such as Butterknife or Retrofit to be searched in any specific repository like `jCenter` or `Jitpack` etc to make sure that we get the right dependency and not the corrupted or fabricated one. This saves us the gradle sync time and results in more reliable code.

Untill Android Studio 3.5, this wasn't possible. But now in Android Studio 3.5, we can do this very easily. Here's how we can do it.

1. Update the Gradle to 5.1 or later in your `gradle-wrapper.properties` file by using the following disribution URL.

```groovy
distributionUrl=https\://services.gradle.org/distributions/gradle-5.1-all.zip
```

2. Now, you can add the exclude groups in for example `jCenter` like this:

```groovy
jcenter {
    content {
        excludeGroup("com.google.firebase")
    }
}
```

This will make sure that all the dependencies with group `com.google.firebase` will NOT be fetched from `jcenter` even if there're available there. We can use other options such as:

* group - `includeGroup("group")` or `excludeGroup("group")`
* module (`group` + `artifact`) - `includeModule("group", "module")` or `excludeModule("group", "module")`
* version (`group` + `artifact` + `version`) - `includeVersion("group", "module", "version")` or `excludeVersion("group", "module", "version")`

But keep in mind that ordering is still important! Let’s take this example where I have declared `jcenter` as the first repository. Then `retrofit2` will be still fetched from `jcenter`: 

```groovy
jcenter()
maven(url = "https://jitpack.io") {
  content {
    includeGroup("com.squareup.retrofit2")
  }
}
```

This is because Gradle tries to resolves a dependency from repo to repo. The `jcenter` is declared first and doesn’t exclude it.
