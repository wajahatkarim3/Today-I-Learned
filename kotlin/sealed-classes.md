# Sealed Classes in Kotlin

Often times, we have a situation where we usually need to manage multiple states of something. For example, our app calls a web service to fetch data from server, then we get any response from server. Now, this response would either be the data we requested, or any error, or no data at all (empty). In these situations, developers have multiple options. They either manage each state (loading, content, empty, error) differently, or they use some kind of flags (```Int``` or ```enum```) to differentiate between states. Either way, there's a risk of wrong condition or parsing data in wrong way. 

Kotlin handles this problem with more easier and more reliable solution by providing ```Sealed Classes```. 
> From the documentation, Sealed classes are used for representing restricted class hierarchies, when a value can have one of the types from a limited set, but cannot have any other type. They are, in a sense, an extension of enum classes: the set of values for an enum type is also restricted, but each enum constant exists only as a single instance, whereas a subclass of a sealed class can have multiple instances which can contain state. 

Enough talk! Let's see a simple example of ```Transportation``` state. 

```kotlin
sealed class Transportation {

    data class Bike () : Transportation()
    data class Car () : Transportation()
    data class Boat () : Transportation()
    data class Aeroplane () : Transportation()
    object NotTransportation : Transportation()
}
```

Now, let's define a simple method for our ```Transportation``` to detect job of each transportation.

```kotlin
fun getJob(transp: Transportation): String = when (transp) {
    is Transportation.Bike -> "Moves on Roads with Speed. Can take maximum 2 persons."
    is Transportation.Car -> "Moves on Roads. Can take upto 4 persons"
    is Transportation.Boat -> "Swims in the water"
    is Transportation.Aeroplane -> "Flies in the air"
    Transportation.NotTransportation -> "Invalid Transportation"
    // else clause is not required as we've covered all the cases
}
```

Though simple example, it can explain a lot. Let's dive into more practical example about the data states (empty, error, loading, content) which we discussed above.

```kotlin
sealed class ServerData {
    data class Error(val message: String) : ServerData(),
    object Empty() : ServerData(),
    object Loading() : ServerData(),
    data class Content(val list: List<String>) : Server Data()
}
```

Please note that we have declared ```Empty``` and ```Loading``` as ```object```s. Its because these will be same in each case, that's why we have defined them as singletons. Now, let's say that your app calls web service. And after the server response, your view is updated accordingly and the method ```updateView``` is called to do that. Below is an example of how this method can be written:

```kotlin
fun updateView(serverData: ServerData)
{
    when(serverData)
    {
        ServerData.Empty -> { myFragment.showEmptyState() }
        ServerData.Loading -> { myFragment.showLoadingState() }
        is ServerData.Error -> { 
            // the serverData variable will be auto type-casted to Error,
            // So you don't have to worry about anything. 
            // You can access error data directly from serverData
            myFragment.showError(serverData.message) 
        }
        is ServerData.Content -> {
            // Similary to error, you can directly access list without any type-casting
            myFragment.updateRecyclerViewContent(serverData.list)
        }
        // else clause is not required as we've covered all the cases
    }
}
```

We can also use ```abstract``` methods as well to make things more simpler. For example, let's put a ```updateViewState()``` method in our ```ServerData``` class like this:

```kotlin
sealed class ServerData {
    data class Error(val message: String) : ServerData(),
    object Empty() : ServerData(),
    object Loading() : ServerData(),
    data class Content(val list: List<String>) : Server Data()
    
    // All methods in sealed classes should be abstract
    abstract fun updateViewState()
}
```

Please note that the methods in any ```sealed``` classes can only be ```abstract```. Now, our each state class should override the ```abstract``` method, ```updateViewState()``` in their own classes. And our whole class will be like this:

```kotlin
sealed class ServerData {
    data class Error(val message: String) : ServerData() {
        override fun updateViewState() {  myFragment.showError(message) }
    }
    object Empty() : ServerData() {
        override fun updateViewState() { myFragment.showEmptyState() }
    }
    object Loading() : ServerData() {
        override fun updateViewState() { myFragment.showLoadingState() }
    }
    data class Content(val list: List<String>) : Server Data() {
        override fun updateViewState() { myFragment.updateRecyclerViewContent(list) }
    }
    
    abstract fun print()
}
```

And now we can just call ```updateViewState()``` on ```ServerData``` directly, and the respective method will be automatically called. 

```kotlin
fun updateView(serverData: ServerData)
{
    // No need of if-else or when here. just call print.
    // In case of Error, error's print method will get called and so on.
    serverData.updateViewState()
}
```
