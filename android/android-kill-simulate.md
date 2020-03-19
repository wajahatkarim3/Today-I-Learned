# How to Simulate Android Kill Process for Your Debug Apps?

Android OS will kill a process if it is in the background and the OS decides if it needs the resources (RAM, CPU, etc.). You need to be able to simulate this behaviour during testing so that you can ensure that your application is behaving correctly. 

Killing the app from terminal is done through this `adb` command:

```
adb shell kill
```

But thid requires process ID and will kill your whole process. So, if you open the app again, it will start from the launcher screen, not the one you left the app and pressed home.

Now for example, when you press home and leave the app in some mid screen with some memory variables etc, and suddenly Android OS requires some resources and it kills your app. Now, when user come back to the app through recent apps option, then Android OS will recreate that opened Activity. This means that if you haven't used any `onSaveState()` and `onRestoreState()` methods, and you have some data which was created in previous Activities. You will get those data variables as null. Because Android OS has only recreated current activity. 

In order to simulate this scenario, you can do it with the following two commands:

```
adb shell am kill <package_name>
```

Please note that this is different than the previous command. The `adb shell kill` kills the whole process of your app while `adb shell am kill` pnly kills processes that are safe to kill -- that is, it will not impact the user experience.

The other command you can use to kill your app like the Android OS when in need of resources does is:

```
adb shell "ps | grep <PACKAGE_NAME> | awk '{print $2}'" | xargs adb shell "run-as <PACKAGE_NAME> kill"
```

Please note that this command uses `xargs` which may not be available in Windows. You can do it with installing [Git for Windows](https://github.com/git-for-windows/git/releases) and running the above command in the Git Bash terminal instead of the normal terminal. Because Git for Windows has more than 200 Linux commands packaged in it.

**Resources**
https://stackoverflow.com/questions/38672888/git-grep-and-xargs-in-windows-batch-file
https://stackoverflow.com/questions/11365301/how-to-simulate-android-killing-my-process
