# Debugging Deep Links of Your App using ADB

Deep links are widely and commonly used in features like user activation, or reset password etc. Today, I was working on such a case about user activation where I wanted to debug a scenario to fix something when user clicked on a link sent in the email. Normally I was manually copying the link from the email in the Messages app of the emulator and clicking from there triggered my app with the deep link detection. 

From the server side, activation link was supposed to be clicked only one time. After that, server returned error. I was trying to debug that one time situation. But my app was always getting error response. After approximately 2 hours of looking into it, I found that Messages app is generating a preview of the URL in the message. This preview was made through calling the URL and that's where this one-time scenario was hit. And when I clicked on the URL, app got the error because it was the second time.

Now, I wanted to hit the URL first time in the app. I found about an ADB command which allowed me to debug the deep links very easily.

Just type this in the terminal first.

```
adb shell am start -W -a android.intent.action.VIEW -d "DEEP_URL_TO_TEST"
```

Please remember that your emulator/device should be connected and that should be only one. In multiple devices, I get an error to specify the device on which to run this command.

After this command, you will see a chooser dialog like this:

<div>
  <img src="https://i.stack.imgur.com/DctZC.png" width="280px" />
</div>

***

Reference: https://www.youtube.com/watch?v=m2h3sK7s2eQ&feature=emb_title
