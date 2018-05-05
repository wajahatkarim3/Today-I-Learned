# IntDef and StringDef in Android

We often use ```View```'s ```visibility``` in our apps to show and hide them. We use ```void setVisibility(int visibility)``` method for that purpose. But have you ever thought that why this method always takes ```VISIBLE```, ```INVISIBLE``` and ```GONE``` rather than any ```int``` value like 0 or 1 etc.? Although method's parameter type is ```int```, then why it doesn't accept the direct numbers or any other ```int``` variable except those three.

In java, ```enum``` is known concept, and in many cases you can use it, but for android, ```enum``` is something you should avoid to use as it’s processing performance is not efficient, so in Android performance patterns it’s told to avoid ```enum```s and to use annotations like ```@IntDef``` and ```@StringDef```.

For example, you are working on a library that make user choose photo from gallery and camera and you are passing ```int``` value in your library method and taking argument in ```int```. Then, based upon that value, you are opening camera or gallery.

```java
class MyPicLibrary
{
    public static void pickPhoto(int source)
    {
        if (source == 0)
        {
            // Camera intent goes here
        }
        else if (source == 1)
        {
            // Gallery intent goes here
        }
    }
}
```

We can create ```static final int``` variables with names like ```CAMERA = 0``` and ```GALLERY = 1``` to make things easier to read and understand, but that will still give developers to pass any kind of ```int``` value in the ```pickPhoto()``` method. But, our goal is to restrict the developers to only pass values through our assigned names as its happening with ```View.setVisibility()``` method.

For this purpose, android has ```@IntDef``` and ```@StringDef``` annotations. Here's a simplest example how previously described problem can be made easy with ```@IntDef```.

```java
class MyPicLibrary
{
      @Retention(RetentionPolicy.SOURCE)                    //declare retention policy source i.e complile time
      @IntDef({OPEN_CAMERA, OPEN_GALLERY})                  // @IntDef value allocation 

      //Declare interface which we gonna use as our custom annotation 
      public @interface ImageSource { }

      // Must be static final int values for IntDef
      public static final int OPEN_CAMERA = 0;
      public static final int OPEN_GALLERY = 1;

      private int source;

      //As setImageSource has argument type of @ImageSource, parameters passed can only be one of both
      // OPEN_CAMERA or OPEN_GALLERY
      public void setImageSource(@ImageSource int source){
          this.source = source;
      }

      @ImageSource
      public int getImageSource(){
          return OPEN_CAMERA || OPEN_GALLERY;   //Here you can't return simple integer value, It will generate compile time error
      }
      
      public static void pickPhoto(@ImageSource int source)
      {
          if (source == OPEN_CAMERA)
          {
              // Camera intent goes here
          }
          else if (source == OPEN_GALLERY)
          {
              // Gallery intent goes here
          }
      }
}
```
******
The important topic here to understand is why we are not using ```enum```s? Although you can use it for similar purposes but they are not as memory efficient as ```static final int``` constants. Here's a nice video of performance optimization discussing the similar topic by Google below.

[![](http://i3.ytimg.com/vi/Hzs6OBcvNQE/maxresdefault.jpg)](https://www.youtube.com/watch?v=Hzs6OBcvNQE)
