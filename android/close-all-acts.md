# Closing All Activities and Launching Any Specific Activity

Often times, most apps have an option where all the activities of the current app are closed and any new specific activity is launched. For example, on logging out of the application, all the activities are closed and login activity is started to allow user to make any new session. This can be done using few lines code with power of intents like this:

### Kotlin

```kotlin
  val i = Intent(applicationContext, SplashActivity::class.java)        // Specify any activity here e.g. home or splash or login etc
  i.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP)
  i.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TASK)
  i.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK)
  i.putExtra("EXIT", true)
  startActivity(i)
  finish()
```

### Java

```java
  Intent i = new Intent(applicationContext, SplashActivity.java);        // Specify any activity here e.g. home or splash or login etc
  i.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP);
  i.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TASK);
  i.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
  i.putExtra("EXIT", true);
  startActivity(i);
  finish();
```
