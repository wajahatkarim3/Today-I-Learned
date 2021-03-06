# Passing Parcelable and Other Data in Navigation Compose


### Normal Arguments
You can pass the usual paramters like `String`, `Float`, `Int` etc through Navigation Arguments. 

In your `NavHost` place, where the whole graph is being defined, you could do like this:

```kotlin
NavHost(startDestination = "profile/{userId}") {
    ...
    composable(
        "profile/{userId}",
        arguments = listOf(navArgument("userId") { type = NavType.StringType })     // Fetching the argument which has been passed
    ) {
        Profile(navController, backStackEntry.arguments?.getString("userId"))       // Using that argument in the destination Composabel
    }
}
```

And here's how the argument is passed in any clickable etc.

```kotlin
navController.navigate("profile/user1234")
```

### Passing Parcelable / Serializable

With the above method, we can't pass any Parcelable or Serializable. So here's a way I found which worked:

In the source screen to pass the data,

```kotlin
navController.currentBackStackEntry?.arguments?.putParcelable("user", userModel)
navController.navigate("userDetails")
```
And in the `NavHost` graph place, here's how you can retrieve it for destination screen

```kotlin
val userModel = navController.previousBackStackEntry?.arguments?.getParcelable<UserModel>("user")
```

Please note that in the sending, we have used `currentBackStackEntry` and in the receiving we have used `previousBackStackEntry`. 

### References
* https://stackoverflow.com/questions/65610003/pass-parcelable-argument-with-compose-navigation
* https://developer.android.com/jetpack/compose/navigation
