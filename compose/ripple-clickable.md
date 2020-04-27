# Ripple vs Clickable Conflict 

I have been experimenting around Jetpack Compose - the new declrative UI library of Android. I am using `0.1.0-dev09` version at the time of writing. So the problem I was having was that my `Clickable`'s `onClick` was not being called. I had structure like this:

```kotlin
  Clickable(
    onClick = {
      // The OnClick Listener
    }
  ) {
    Box(
        modifier = Modifier.fillMaxWidth().padding(end = 10.dp) + Modifier.ripple(bounded = false),
      ) {
        // Content of the screen
      }
  }
```

In this structure, the `onClick` was not being called no matter what. After reading through the code in Jetpack Compose, I realized that the `Ripple` should be directly in the `Composable` which is triggering `onClick`. So, in this example, the `Ripple` should be moved from the `Box` modifier to `Clickable` modifier like this:

```kotlin
  Clickable(
    onClick = {
      // The OnClick Listener
    },
    modifier = Modifier.ripple(bounded = false)             // <--- NOTE THIS LINE
  ) {
    Box(
        modifier = Modifier.fillMaxWidth().padding(end = 10.dp),
      ) {
        // Content of the screen
      }
  }
```

And that's how you can add a ripple effect with click.
