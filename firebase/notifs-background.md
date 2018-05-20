# Firebase Notifications in Background & Foreground in Android

When developers need to integrate push notifications in their Android apps, the first thing comes in mind is [Firebase Notifications or Firebase Messaging](https://firebase.google.com/docs/cloud-messaging/). But when they integrate, there are few common issues which they stuck into. One of those common issues is that the applications doesn't get push notifications. There are multiple reasons such as ```Unregistered device token``` or app is killed or Android Oreo's doze mode is on etc.

We will talk about here for one reason of app being in background and foreground and see how it can be solved. Firebase Notifications applies different mechanisms when app is foreground and when app is in background.

### Handling Notifications in Foreground
When the app is closed, your notifications are processed by the Google Service process, which take care of displaying your notifications as required, including the default click action (opening the app) and the notification icon.

When the app is in foreground, the received messages are processed by the app, and since there’s no logic to handle it, nothing will happen!

To fix this, we need our own ```FirebaseMessagingService```, let’s create one. Create a new class that extends it and implement the ```onMessageReceived``` method. Then get the ```Notification``` object from the ```remoteMessage``` and create your own Android Notification.

```java
public class NotificationService extends FirebaseMessagingService { 
  @Override
  public void onMessageReceived(RemoteMessage remoteMessage) {
    super.onMessageReceived(remoteMessage);
    Notification notification = new NotificationCompat.Builder(this)
      .setContentTitle(remoteMessage.getNotification().getTitle())
      .setContentText(remoteMessage.getNotification().getBody())
      .setSmallIcon(R.mipmap.ic_launcher)
      .build();
     NotificationManagerCompat manager = NotificationManagerCompat.from(getApplicationContext());
     manager.notify(123, notification);
   }
}
```

Then add the ```Service``` in your ```AndroidManifest.xml```

```xml
<service android:name=”.NotificationService”>
 <intent-filter>
  <action android:name=”com.google.firebase.MESSAGING_EVENT”/>
 </intent-filter>
</service>
```

Now if you try again, you will display notifications while your app is in foreground!

In real life, your ```onMessageReceived``` content will be slightly more complex, you will want different smart actions depending on the type of notification, you will want to show a nicer large icon (the one that appears on the notification body) and for sure to change the status bar icon.

The problem you have now is that your ```onMessageReceived``` is ONLY called when the app is in foreground, if you app if is background, the Google Services will take care of displaying your message.

The solution? don’t use the ```“notification”``` message payload and use ```“data”``` instead.

### Using Data

The last step to solve this foreground/background problem is to ditch the ```notification``` object from your message payload.

Rather than sending:

```json
{
   "to": "the device token"
   "notification":{
     "title":"New Notification!",
     "body":"Test"
   },
   "priority":10
 }
```

Send:

```json
{
   "to": "the device token"
   "data":{
     "title":"New Notification!",
     "body":"Test"
   },
   "priority":10
 }
 ```
 
This way, your notifications will be always handled by the app, by your NotificationService, and not by the Google Service process.

You will need to change your code as well to handle the data payload:

```java

    // Old Way
    //.setContentTitle(remoteMessage.getNotification().get(“title”))
    //.setContentText(remoteMessage.getNotification().get(“body”))

    // New Way
    .setContentTitle(remoteMessage.getData().get(“title”))
    .setContentText(remoteMessage.getData().get(“body”))
```

Your whole ```NotificationService``` will look like this now:

```java
public class NotificationService extends FirebaseMessagingService { 
  @Override
  public void onMessageReceived(RemoteMessage remoteMessage) {
    super.onMessageReceived(remoteMessage);
    Notification notification = new NotificationCompat.Builder(this)
      .setContentTitle(remoteMessage.getData().get(“title”))
      .setContentText(remoteMessage.getData().get(“body”))
      .setSmallIcon(R.mipmap.ic_launcher)
      .build();
     NotificationManagerCompat manager = NotificationManagerCompat.from(getApplicationContext());
     manager.notify(123, notification);
   }
}
```

One more advantage of using ```data``` instead of ```notification``` object is that you can put anything you want in the ```“data”``` object. For example a user ID, a URL to an image… any information that you might want to use to build the notification or to pass to the click action. Note that all will be treated as ```String```s. For example,

```json
"data":{
     "title":"New Notification!",
     "body":"Test",
     "user_id": "1234",
     "avatar_url": "http://www.example.com/avatar.jpg"
 },
```

This difference is also explained in the Handling Messages section of this document: https://firebase.google.com/docs/cloud-messaging/android/receive

## Conclusion
Note that if you keep the ```“notification”``` object in your payload, it will behave just like before. You need to get rid of the ```“notification”``` and only provide the ```“data”``` object.
