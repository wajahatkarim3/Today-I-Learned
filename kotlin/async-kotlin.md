# Asynchronous Calls in Koltin

A very common task in any android app is to hit any web service call and fetch some data from server. Google have always recommended to perform such heavy or time consuming calls in background thread, as if performed in main thread, these can block the UI and make the app irresponsive Traditionaly, developers use ```AsyncTask``` for these sorts of computations. But working with ```AsyncTask``` is not only complex and lengthy, but also very frustrating. 

This is a simple example of ```AsyncTask``` below:

```java
class MainActivity extends AppCompatActivity
{
    @Override
    public void onCreate(Bundle savedInstanceState)
    {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        
        Button btndownload = findViewById(R.id.btnDownload);
        //Notice that I didn't need to call findViewById(), thanks to Synthetic Binding!
        btndownload.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                
                downloadData();
            }
        });
    }
    
    public void downloadData()
    {
        new TestAsync().execute();
    }
}

class TestAsync extends AsyncTask<Void, Integer, String>
{
    String TAG = getClass().getSimpleName();

    protected void onPreExecute (){
        super.onPreExecute();
        Log.d(TAG + " PreExceute","On pre Exceute......");
    }

    protected String doInBackground(Void...arg0) {
        Log.d(TAG + " DoINBackGround","On doInBackground...");

        for(int i=0; i<10; i++){
            Integer in = new Integer(i);
            publishProgress(i);
        }
        return "You are at PostExecute";
    }

    protected void onProgressUpdate(Integer...a){
        super.onProgressUpdate(a);
        Log.d(TAG + " onProgressUpdate", "You are in progress update ... " + a[0]);
    }

    protected void onPostExecute(String result) {
        super.onPostExecute(result);
        Log.d(TAG + " onPostExecute", "" + result);
    }
}
```

So, if we are working on any data driven app, then for each web service call like on performing login, on getting social feed, on clicking on like button, on adding any new post, etc. we have to write all this code, which will not only make our code unreadable and complex but also very difficult to manage and test.

Kotlin comes with a solution for this using its DSL features. First you have to do is add Anko library to your gradle.

```groovy
implementation "org.jetbrains.anko:anko-commons:LATEST_VERSION"
```

Now, we can perform this operation very easily like this:

```kotlin
class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        
        //Notice that I didn't need to call findViewById(), thanks to Synthetic Binding!
        btndownload.setOnClickListener { downloadData() }
    }

    fun downloadData() {
        doAsync {
            //Execute all the long running tasks here
            val s: String = makeNetworkCall()
            uiThread {
                //Update the UI thread here
                alert("Downloaded data is $s", "Hi I'm an alert") {
                    yesButton { toast("Yay !") }
                    noButton { toast(":( !") }
                }.show()
            }
        }
    }
}
```

That’s it!

No need to create a custom class and override all those gnarly methods just to run a long running task and update the UI from there.

Note that ```doAsync``` also aware of the underlying ```Activity```/```Fragment```’s lifecyle and hence if the original ```Activity```/```Fragment``` which started this block is no longer alive (recreated because of configuration changes), the code inside ```uiThread``` block isn’t executed!

This greately reduces the Memory Leaks that happen in case of ```AsyncTask```s without us having to worry about it!
