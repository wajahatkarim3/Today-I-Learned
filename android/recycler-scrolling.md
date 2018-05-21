# RecyclerView Scrolling Issue with NestedScrollView

Although ```RecyclerView``` has a very good and smooth scrolling built-in, but when you put into any ```ScrollView```, then your ```RecyclerView```'s scrolling will not work. For example,

```xml
<ScrollView
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="horizontal">

        <android.support.v7.widget.RecyclerView
            android:id="@+id/listRecyclerView"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:layout_marginBottom="@dimen/_10sdp"
            tools:listitem="@layout/activity_type_item_layout"/>

    </LinearLayout>

</ScrollView>
```

Now, the ```RecyclerView``` will not scroll. To fix that, you will have to use ```NestedScrollView``` instead of ```ScrollView``` like this:

```xml
<android.support.v4.widget.NestedScrollView
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="horizontal">

        <android.support.v7.widget.RecyclerView
            android:id="@+id/listRecyclerView"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:layout_marginBottom="@dimen/_10sdp"
            tools:listitem="@layout/activity_type_item_layout"/>

    </LinearLayout>

</android.support.v4.widget.NestedScrollView>
```

Now, the ```RecyclerView``` will scroll but it will not be smooth. It will stop as soon as finger is off the screen resulting in a very stucking scrolling. To fix this issue, all you have to do is add this line after ```RecyclerView```s adapter has been set.

```java
ViewCompat.setNestedScrollingEnabled(listRecyclerView, false);
```

Now, your ```RecyclerView``` will work with smooth scrolling. This same technique can be used in ```ViewPager``` when each ```Fragment``` have ```RecyclerView``` and the ```ViewPager``` is inside any ```CollapsingToolbarLayout```. 
