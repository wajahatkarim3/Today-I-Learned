# Idling Registry for OkHttp

A lot apps use Retrofit or OkHttp for the networking, and when it comes to testing the networked operations, then it becomes a little tricky. As network calls can take a little time, and test fails early due to the delay. In such cases, [Idling Registry](https://developer.android.com/reference/android/support/test/espresso/IdlingRegistry) is used for testing. 

If you are using Retrofit or OkHttp, then there's this [OkHttp Idling Resource](https://github.com/JakeWharton/okhttp-idling-resource) which makes testing idling resources and network calls a lot more easier with few lines of code. 

For example, we have a simple login service, which returns ```success``` value in case of successful login and displays it on a ```TextView``` with ```txtStatus``` ID. So, the test might be something like this.

```java
    @Test
    public void testLogin()
    {
        onView(withId(R.id.txtStatus))
            .check(matches(withText("success")));
    }
```

Now, when we run the test, it will fail. Even if there is successful login. The reason for this is that unit test is faster than the network call and it is not waiting for the response from server. And that's why it fails. So, we need our test to wait for the response or we need our test to be idle until some response is returned from the network call. This is where idling registry and idling resources are useful. 

### OkHttp Idling Resource
You can read more about OkHttp Idling Resource at [here](https://github.com/JakeWharton/okhttp-idling-resource). We will add two dependencies, one will be for Espresso Idling Registry and other will be for OkHttp Idling Resource.

```groovy
    debugImplementation 'com.android.support.test.espresso.idling:idling-concurrent:3.0.0'
    debugImplementation 'com.jakewharton.espresso:okhttp3-idling-resource:1.0.0'
```

Now, we need to create idling resource. So let's create ```IdlingResources.java``` class. 

```java
public abstract class IdlingResources
{
    public static void registerOkHttp(OkHttpClient client)
    {
        IdlingRegistry.getInstance().register(OkHttp3IdlingResource.create("okhttp", client));
    }
}
```

Since, you don't want to put this class in ```release``` build, so its a good practice to create same class with empty function body in the release package using android flavors. This dummy class can look like this.

```java
public abstract class IdlingResources
{
    public static void registerOkHttp(OkHttpClient client)
    {
        // Dummy and Empty Body
    }
}
```

Now, when we are setting up our ```Retrofit``` instance, we can setup this resource for ```OkHttp``` there like this:

```java
    OkHttpClient client = new OkHttpClient();
    IdlingResources.registerOkHttp(client);         // THIS IS THE IMPORTANT LINE
    Retrofit retrofit = new Retrofit.Builder()
        .baseUrl("https://my.base.url/")
        .addConverterFactory(MoshiConverterFactory.create())
        .client(client)
        .build();
```

Now, when we run the test, they will be passed.
