# The requireActivity() and requireContext() example

When we use ```Fragment``` in our app, we often time need access to ```Context``` or ```Activity```. We do it by calling methods such as ```getContext()``` and ```getActivity()``` methods. But, in kotlin, these methods return nullables and we end up using code like this.

```kotlin
    fun myTempMethod()
    {
        context?.let {
            // Do something here regarding context
        }
        
        // Or we do it like this
        var myNonNullActivity = activity!!
    }
```

For example, we need ```Activity``` in asking permissions. So, with using above code approach, this will be:

```kotlin
    fun askCameraPermission()
    {
        PermissionUtils.requireCameraPermission(activity!!, REQUEST_CODE_CAMERA)
    }
```

Now, this code is very bad. When ```Fragment``` is not attached to any ```Activity```, this code will crash and throw ```NullPointerException```. Some developers avoid this by using ```as``` operator.

```kotlin
    fun askCameraPermission()
    {
        PermissionUtils.requireCameraPermission(activity as Activity, REQUEST_CODE_CAMERA)
    }
```

But, this is also almost same as bad as the previous example. Luckily, in Support Library 27.1.0 and later, Google has introduced new methods ```requireContext()``` and ```requireActivity()``` methods. So, we can do above example like this now:

```kotlin
    fun askCameraPermission()
    {
        PermissionUtils.requireCameraPermission(requireActivity(), REQUEST_CODE_CAMERA)
    }
```

Now, this method will make sure that ```Fragment``` is attached and returns a valid non-null ```Activity``` which we can use without any trouble.
