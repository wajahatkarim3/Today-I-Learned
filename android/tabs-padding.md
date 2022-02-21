# Adding Padding in Tabs in Android

In the app, which I am making at my job, I had a situation where I needed a huge number of tabs with the ```Fragment View Pager```. I used a typical ```TabLayout``` and ```ViewPager``` with a custom ```FragmentPagerAdapter``` class as the adapter for ```ViewPager```.

This is ```TabLayout``` and ```ViewPager``` in ```XML``` layout.

```xml
<Relative Layout>

    <android.support.design.widget.AppBarLayout>
    
        <android.support.v7.widget.Toolbar/>
        
        <android.support.design.widget.TabLayout
                android:id="@+id/tabs"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_gravity="center_horizontal"
                app:tabMode="scrollable"/>
    
    </<android.support.design.widget.AppBarLayout>

    <android.support.v4.view.ViewPager
            android:id="@+id/viewpager"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            app:layout_behavior="@string/appbar_scrolling_view_behavior"  />

</Relative Layout>

```

But, as I run it, this is how it looked like:

<img src="https://raw.githubusercontent.com/wajahatkarim3/Today-I-Learned/master/android/resources/tabs_before_padding.png" data-canonical-src="https://raw.githubusercontent.com/wajahatkarim3/Today-I-Learned/master/android/resources/tabs_before_padding.png" width="300" />

But, in Android Support library, there are two attributes for ```TabLayout``` which can fix this issue very easily. Just add these two lines in ```TabLayout``` and you're done!

```xml
app:tabPaddingStart="10dp"
app:tabPaddingEnd="10dp"
```

Now, when you run, app will show like this:

<img src="https://raw.githubusercontent.com/wajahatkarim3/Today-I-Learned/master/android/resources/tabs_after_padding.png" data-canonical-src="https://raw.githubusercontent.com/wajahatkarim3/Today-I-Learned/master/android/resources/tabs_after_padding.png" width="300" />
