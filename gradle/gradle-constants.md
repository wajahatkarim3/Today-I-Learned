# Defining Constants in Gradle

Every android app has some global constants values such as Base URL, API keys etc. Usually, developers define these constants in any ```AppConstants.java``` file as ```static final``` variables, or they define these in ```strings.xml``` file.

But, in Android, there's a better and more secure way for this. You can define the constants in app's ```build.gradle``` file like this:

```groovy
apply plugin: 'com.android.application'

// ...

android {
  // ... 
  
  buildTypes {
    debug {
      // ...
      buildConfigField "String", "SERVER_URL", "http://mybase.url.com/debug/"
    }
    release {
      // ...
      buildConfigField "String", "SERVER_URL", "http://mybase.url.com/release/"
    }
  }
}

// ...

```

Now, you can access it in the code like this:

```kotlin
var myUrl = BuildConfig.SERVER_URL
// Do stuff with myUrl here
```
