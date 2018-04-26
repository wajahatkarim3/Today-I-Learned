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

The same idea of structure the use of API in that way can be applied also on single class with specific responsibility. These are some examples to learn more about it [here for ```ArtilceBuilder``` class](https://proandroiddev.com/kotlin-dsl-everywhere-de2994ef3eb0) and [here for ```Person``` class](https://proandroiddev.com/writing-dsls-in-kotlin-part-1-7f5d2193f277). 

Let's discuss here another practical example of using [Retrofit](http://square.github.io/retrofit/) for calling any API in android with kotlin.

### The Normal Way Of Retrofit Call

```kotlin
var call = retrofit.create(MyApiService::class.java).anyTestApi("param1", "param2")
  call.enqueue(object : Callback<String>{
    
        override fun onResponse(call: Call<String>?, response: Response<String>?) {
            // Do anything with response here
        }
        
        override fun onFailure(call: Call<String>?. throwable: Throwable?){
            throwable?.printStackTrace()
            // Do anything here with error
        }
  })
```

### The DSL-Way of Retrofit Call

```kotlin
   var call = retrofit.create(MyApiService::class.java).anyTestApi("param1", "param2")
    call.enqueue( RetrofitCallback {
        
        onSuccessCallback { call, response ->
            // Do anything with response here
        }
        
        onFailureCallback { call, throwable ->
            throwable?.printStackTrace()
            // Do anything here with error
        }
    })
```

## Show Me The Code!!! 
Let's start by creating a class ```RetrofitCallback``` which will implement the original ```Callback<T>``` from Retrofit library and override the methods as well the  like this:

```kotlin
class RetrofitCallback<T> : Callback<T> {
  
  override fun onFailure(call: Call<T>?, t: Throwable?) {
    
  }

  override fun onResponse(call: Call<T>?, response: Response<T>?) {
    
  }
  
}
```
Now, let's create our own callback variable references which will be called in DSL way and these are the blocks to be invoked. 

```kotlin
class RetrofitCallback<T> : Callback<T> {
  
  private var _failureCallback: (call: Call<T>?, throwable: Throwable?) -> Unit = { _, _ -> }
  private var _successCallback: (call: Call<T>?, response: Response<T>?) -> Unit = { _, _ -> }

  // ... Rest of the class code
}
```

Finally, assign these variables values (function references or higher order functions) in kotlin like this:

```kotlin
class RetrofitCallback<T> : Callback<T> {
  
  // ... Rest of the class code
  
  fun onFailureCallback( function: (call: Call<T>?, throwable: Throwable?) -> Unit)
  {
      _failureCallback = function
  }

  fun onSuccessCallback( function: (call: Call<T>?, response: Response<T>?) -> Unit)
  {
      _successCallback = function
  }
  
}
```

Finally, add an ```init``` method to start the calls on constructor and call these methods in our original callback methods and final code will be like this:

```kotlin
class RetrofitCallback<T>(initMethod: RetrofitCallback<T>.() -> Unit) : Callback<T> {

    private var _failureCallback: (call: Call<T>?, throwable: Throwable?) -> Unit = { _, _ -> }
    private var _successCallback: (call: Call<T>?, response: Response<T>?) -> Unit = { _, _ -> }

    override fun onFailure(call: Call<T>?, t: Throwable?) {
        onFailureCallback(call, t)
    }

    override fun onResponse(call: Call<T>?, response: Response<T>?) {
        onSuccessCallback(call, response)
    }

    fun onFailureCallback( function: (call: Call<T>?, throwable: Throwable?) -> Unit)
    {
        _failureCallback = function
    }

    fun onSuccessCallback( function: (call: Call<T>?, response: Response<T>?) -> Unit)
    {
        _successCallback = function
    }

    init {
        initMethod()
    }
}
```

Please note that how ```initMethod``` is passed in ```RetrofitCallback``` constructor. This is where all the DSL calls get invoked. And voila! We are done. You can customize it more by adding custom methods such as ```onEmptyCallback```, or ```on404Callback``` or anything else.
