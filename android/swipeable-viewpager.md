# Enable/Disable Swipe of ViewPager or Swipeable ViewPager

When creating image gallery with Zoom/Pan support in `ViewPager`, the touch events of zoom/pan often conflict with `ViewPager`'s swipe events and it makes a confusing user experience. So, in those cases, you need to enable/disable swipe of `ViewPager` based on the scale or position of the image, for example. 

Add this custom `ViewPager` in your project first.

```java
public class SwipeableViewPager extends ViewPager {

    private boolean swipeable;

    public SwipeableViewPager(Context context, AttributeSet attrs) {
        super(context, attrs);
        TypedArray a = context.obtainStyledAttributes(attrs, R.styleable.swipeable_view_pager);
        try {
            swipeable = a.getBoolean(R.styleable.swipeable_view_pager_svp_swipeable, true);
        } finally {
            a.recycle();
        }
    }

    @Override
    public boolean onInterceptTouchEvent(MotionEvent event) {
        return swipeable ?
                super.onInterceptTouchEvent(event) : false;
    }

    @Override
    public boolean onTouchEvent(MotionEvent event) {
        return swipeable ?
                super.onTouchEvent(event) : false;
    }

    public boolean isSwipeable() {
        return swipeable;
    }

    public void setSwipeable(boolean swipeable) {
        this.swipeable = swipeable;
    }
}
```

Now add this `attrs.xml` in your `res/values` directory.

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <declare-styleable name="swipeable_view_pager">
        <attr name="svp_swipeable" format="boolean" />
    </declare-styleable>
</resources>
```

Now you can either enable/disable swipe from directly your XML layouts like this:

```xml
    <com.app.package.name.SwipeableViewPager
            android:id="@+id/viewPagerGallery"
            android:layout_width="match_parent"
            android:layout_height="0dp"
            app:swipeable="true"                                  <!-- This line is Important -->
            app:layout_constraintEnd_toEndOf="parent"
            app:layout_constraintStart_toStartOf="parent"
            app:layout_constraintTop_toTopOf="parent" />
```

Or you can enable/disable swipe from Java/Kotlin like this:

```kotlin
        viewPagerGallery.isSwipeable = value
```

And now your image zoom/pan touch events won't intercept with the `ViewPager` swipe events.
