# Gradle doesn't run because it can't find tools.jar in JRE 

Today, when I was uploading new version of [EasyFlipView](https://github.com/wajahatkarim3/EasyFlipView) library on ```jCenter```, then I found this strange error which was not rebuilding my whole library module. The error was

```
Execution failed for task ':app:compileDebugJavaWithJavac'.
    Could not find tools.jar. Please check that C:\Program Files\Java\jre1.8.0_121 contains a valid JDK installation.
```

I tried googling around and found answers on StackOverflow like [this](https://stackoverflow.com/questions/11345193/gradle-does-not-find-tools-jar) and [this](https://stackoverflow.com/questions/47291056/could-not-find-tools-jar-please-check-that-c-program-files-java-jre1-8-0-151-c). I tried different solutions. Here's the one which was very simple and worked for me in an instant.

### Solution

Add this to ```gradle.properties```:

```groovy
org.gradle.java.home=C:\\Program Files\\Java\\jdk1.8.0_91
```

Don't forget to use double back slashes. For example: ```org.gradle.java.home=C:\\Program Files\\Java\\jdk1.8.0_144\``` . 
