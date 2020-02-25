# Displaying the Dependency Graph of whole project

Sometimes we need to see which libraries/dependencies are using which other sub-dependencies and which of their versions are being used. For that, we often require the whole dependency graph of the project. 

Luckily, Android Studio provides us a way to do that. Here's how:

## Through Android Studio Gradle Panel

Click on the `Gradle` tab and then double click on `:yourmodule` -> `Tasks` -> `android` -> `androidDependencies`. This will print the whole graph in your Android Studio console like the image below:

![](https://i.stack.imgur.com/uInQi.jpg)

***

## Through Depenendencies Command
In the Android Studio Terminal, or your PC's terminal, you can directly put this command and it will print the graph in the terminal.

```groovy
./gradlew yourmodule:dependencies
```
Additionally, if you want to check if something is `compile`, `testCompile`, `androidTestCompile`, `implementation`, `testImplementation`, or `androidTestImplementation` dependency as well, you can do so with the `configuration` parameter like this:

```groovy
./gradlew yourmodule:dependencies --configuration implementation
./gradlew yourmodule:dependencies --configuration testImplementation 
./gradlew yourmodule:dependencies --configuration androidTestImplementation 
```

***

## Through Project Report
If you find it hard to navigate console output of gradle dependencies, you can add the [Project reports plugin](https://docs.gradle.org/current/userguide/userguide_single.html#project_reports_plugin). Put this in your app's `build.gradle` file on top:

```groovy
apply plugin: 'project-report'
```

And run this command in Android Studio Terminal:

```
gradlew htmlDependencyReport
```

And this will show you a nice report with dependencies like this image:

![](https://i.stack.imgur.com/mIybP.png)

***

### Through Build Scan

Since Gradle 4.3, "build scans" were introduced. All relevant info is available in the Gradle docs ([1](https://guides.gradle.org/creating-build-scans/), [2](https://scans.gradle.com/)). This seems to now be the easiest way to check your dependencies (and generally your build) in a clear, organized way.

They are very easy to create, just execute: 
```groovy
gradlew build --scan
```

Or if you want to just focus on dependencies, you can simply do this:

```groovy
gradlew app:dependencies --scan
```

After the command ends, you will get a message like this:

```
Publishing a build scan to scans.gradle.com requires accepting the Gradle
Terms of Service defined at https://gradle.com/terms-of-service. Do you
accept these terms? [yes, no]
```

Enter `yes` in your terminal, and you will be shown a URL like this:

```
Publishing build scan...  
https://gradle.com/s/a12en0dasdu
```

Open this URL, enter your email. And in your email, you will get a build. Opening the build will show you a full report of your task like this:

![](https://i.stack.imgur.com/ybLD5.png)

***

Reference: https://stackoverflow.com/questions/21645071/using-gradle-to-find-dependency-tree
