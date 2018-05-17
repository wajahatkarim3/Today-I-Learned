# Launching Activities using Kotlin DSL

Launching activities in android apps is a common task and different developers use different approaches. Some use the traditional ways of creating ```Intent``` bundles and passing them in ```startActivity()``` methods along side the ```Intent```s. But, with Kotlin DSL and writing a handful of extension methods, this can become a lot more easier and readable than the traditional ways. 

Let's look at the typical traditional way of starting another activity

```kotlin
    var intent = Intent(myActivity, TargetActivity::class)
    myActivity.startActivity(intent)
```

Now, if we pass some arguments in it, this becomes more complex.

```kotlin
    var intent = Intent(myActivity, TargetActivity::class)
    intent.putExtra("myIntVal", 10)
    intent.putExtra("myStrVal", "Hello String")
    intent.putExtra("myBoolVal", false)
    myActivity.startActivity(intent)
```

But, creating some kotlin extension methods and applying DSL rules, we can make these a lot simpler.

Add these methods in any class of your kotlin class, ideally  in any extension class:

```kotlin

/**
 * Extensions for simpler launching of Activities
 */

inline fun <reified T : Any> Activity.launchActivity(
        requestCode: Int = -1,
        options: Bundle? = null,
        noinline init: Intent.() -> Unit = {}) {

    val intent = newIntent<T>(this)
    intent.init()
    if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.JELLY_BEAN) {
        startActivityForResult(intent, requestCode, options)
    } else {
        startActivityForResult(intent, requestCode)
    }
}

inline fun <reified T : Any> Context.launchActivity(
        options: Bundle? = null,
        noinline init: Intent.() -> Unit = {}) {

    val intent = newIntent<T>(this)
    intent.init()
    if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.JELLY_BEAN) {
        startActivity(intent, options)
    } else {
        startActivity(intent)
    }
}

inline fun <reified T : Any> newIntent(context: Context): Intent =
        Intent(context, T::class.java)
```

Now, after this, we can do same launching activity stuff (of previous examples) like this:

```kotlin
    myActivity.launchActivity<TargetActivity> {  }
```

Or we can pass arguments like this:

```kotlin
    myActivity.launchActivity<TargetActivity> { 
        putExtra("myIntVal", 10)
        putExtra("myStrVal", "Hello String")
        putExtra("myBoolVal", false)
    }
```

Here are all the cases which you can do with these extensions:

```kotlin
    // super simple, no arguments for simple Activities but doesn't prevent a crash for this one
    launchActivity<UserDetailActivity>()

    // add the required argument
    launchActivity<UserDetailActivity> {
        putExtra(INTENT_USER_ID, user.id)
    }

    // add custom flags
    launchActivity<UserDetailActivity> {
        putExtra(INTENT_USER_ID, user.id)
        addFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP)
    }

    // use options for cool animations
    val options = ActivityOptionsCompat.makeSceneTransitionAnimation(this, avatar, "avatar")
    launchActivity<UserDetailActivity>(options = options) {
        putExtra(INTENT_USER_ID, user.id)
    }

    // requestCode, why not!?
    launchActivity<UserDetailActivity>(requestCode = 1234) {
        putExtra(INTENT_USER_ID, user.id)
    }
```
