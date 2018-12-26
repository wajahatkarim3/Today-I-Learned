# Customizing the Scroll Duration of ViewPager

First of all we need to extract the field `mScroller` from the `ViewPager` class  so that we can start making changes on it.

```java
Field mScroller = ViewPager.class.getDeclaredField("mScroller");
```

Now that we have the scroller we could in theory extract the field `mDuration` from it and apply our own preferred value, but unfortunately that will not work. The `ViewPager` explicitly passes the duration to the `mScroller` when requesting to scroll according to some calculations that I am not going to explain here. As a matter of fact, our duration value will be ignored. So we need to work around this limitation.

We can create our own `CustomScroller` by extending the `Scroller` class, and then associate it to the `ViewPager`.

```java
mScroller.setAccessible(true);
mScroller.set(mPager, new CustomScroller(duration));
```

In this way we can define our preferred duration in the `CustomScroller` and use it in the overridden method `startScroll()` as shown below:

```java
public class CustomScroller extends Scroller {

    private int mDuration;

    public CustomScroller(Context context, Interpolator interpolator, int duration) {
        super(context, interpolator);
        mDuration = duration;
    }

    @Override
    public void startScroll(int startX, int startY, int dx, int dy, int duration) {
        // Ignore received duration, use fixed one instead
        super.startScroll(startX, startY, dx, dy, mDuration);
    }
}
