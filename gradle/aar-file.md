# Where to get AAR file of any library?

At my current project at work, I have a situation to add a third-party library locally instead of fetching it from ```jCenter``` or ```JitPack.io```. So, my first task is to download ```AAR``` or ```JAR``` file of the library. The reason of using ```AAR/JAR``` is to avoid compiling of library on each build of the project to decrease build time.

### Here's how
First, you need to include it in your project using traditional way of ```implementation``` as you do with Android Studio. Once its compiled, then go to the directory:

```C:\Users\ USER_NAME \.gradle\caches\modules-2\files-2.1\ LIBRARY_PACKAGE_NAME \ MODULE_NAME \ VERSION \```

Here, you can see 1 or more directories with long key hashe names. Each directory will contain either ```AAR``` or ```JAR``` or ```POM``` file. Copy the ```AAR``` file and paste it in your ```app```'s ```libs``` directory. Ant then add this line in your app level ```build.gradle``` file.

```groovy
implementation ('libs/LIBRARY_AAR_FILE_NAME_WITH_EXTENSION') {
        transitive true
    }
```
