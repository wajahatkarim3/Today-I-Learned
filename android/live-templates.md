# Live Code Templates in Android Studio

Code completion can improve your productivity by reducing how much you have to type, but there are situations when a more powerful tool is needed. Thanks to Android Studio and IntelliJ, live templates make it much easier to focus on just the things you care about. You guessed it right. I am talking about **Live Templates**.

For example, to show a simple ```Toast``` in android apps, we use something like this:

```java
Toast.makeText(context, "The Message For The Toast");
```

Each time, if we want ```Toast```, we have to write exactly the same code with only ```Context``` and ```message``` different. Android Studio allows us to create templates or as they are called *Live Templates*, through which we can write same code with minor differences much more quicker than typing it all as shown in the picture below:

![](https://cdn-images-1.medium.com/max/1000/1*JkrYXGs1AxZAbK0sCLrJAQ.gif)

As you can see, Live Templates are shortcuts displayed as code-completion options that, when selected, insert a code snippet that you can tab through to specify any required arguments.

For example, as shown above — typing “Toast” then hitting the ```Tab``` key inserts the code for displaying a new ```Toast``` with argument placeholders that you can enter, before hitting tab and moving on to the next argument. For the ```Toast``` example above, we can do it using this line of code below:

```java
android.widget.Toast.makeText($className$.this, "$text$", android.widget.Toast.LENGTH_SHORT).show(); 
```

To add this code for *Live Templates* in Android Studio, go to ```File``` -> ```Settings``` -> ```Editor``` -> ```Live Templates```, and you will see all the existing templates by Android Studio and your custom templates as well like the image below.

![](https://cdn-images-1.medium.com/max/1000/1*Y3RLbdNEfoRpf0RU54U9vw.png)

Then you can create new template and paste the above line and you're done. You can also change the keywords for already existing templates as well like this:

![](https://cdn-images-1.medium.com/max/1000/1*Un6kiAIOCGn_lwPtn3ln-w.gif)

You can learn more about templates on this video from Google.

[![Everything Is AWESOME](https://img.youtube.com/vi/4rI4tTd7-J8/0.jpg)](https://youtu.be/4rI4tTd7-J8?t=35s "Everything Is AWESOME")
