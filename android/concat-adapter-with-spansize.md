# Concat Adapter and GridLayoutManager Span Size

Today, I encountered a situation where I had a Grid list of 2 columns with pagination. Now, I wanted to add an extra item at the end of the list where pagination stopped. I did it through the usage of `ConcatAdapter`. 

First, I have two `RecyclerView.Adapter` classes. One of the items of the Grid and other for the Footer. I had to create a single item (`getItemCount() = 0`) Adapter class to add this through `ConcatAdapter`. If I put these directly inside `NestedScrollView` along with the `RecyclerView`, this will create lagging scrolling experience and also will create issues for Pagination.

And then in the `Fragment` class, I had something like this:

```kotlin
class MyFragment: Fragment() {
  // .... Other methods
  
  override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
    super.onViewCreated(view, savedInstanceState)
    
    val itemsAdapter = MyItemsAdapter(itemsList)
    val footerAdapter = MyFooterAdapter()
    
    // I can pass as many Adapters in ConcatAdapter class. But I am passing only itemsAdpater 
    // because footerAdapter will be reached when the list reaches last page. 
    val concatAdapter = ConcatAdapter(itemsAdapter)
    binding.recyclerView.adapter = concatAdapter
    
    // Add OnScrollListener for Pagination or Paging 3 API or whatever
    
  }
}
```

Now, once we reach to the last page and there's no more pagination, then we can call this method to add Footer in the list.

```kotlin
fun showHideFooter(showFooter: Boolean) {
  if (showFooter) {
    // Add Footer in the list
    lastPageReached = true
    
    // Add only if there's no instance already added in the list
    if (!concatAdapter.adapters.contains(footerAdapter)) {
        concatAdapter.addAdapter(footerAdapter)
    }
    else {
        // Remove Footer from the list
        lastPageReached = false
        concatAdapter.removeAdapter(footerAdapter)
    }
  }
}
```

Now this plays really nicely when there's simple `LinearLayoutManager`. But, if I have a `GridLayoutManager` with `spanSize` of 2, then this shows 2 Items in a single row. So, if the list have any odd number of items like 13, then the `footerAdapter` will be added in the 14th item and thus its size will be half in width. So, I wanted to show the Footer with full width size, so I used the `SpanSizeLookup` like this:

```kotlin
    /**
     * Returns an instance of the [GridLayoutManager] with the
     * Span Size calculations to allow the Footer
     * to allocate full width of the screen utilizing 2 Grid Cells.
     */
    fun getFooterAdjustedGridLayoutManager(context: Context): GridLayoutManager {
        val layoutManager = GridLayoutManager(context, 2)
        layoutManager.orientation = LinearLayoutManager.VERTICAL
        layoutManager.spanSizeLookup = object : GridLayoutManager.SpanSizeLookup() {
            override fun getSpanSize(position: Int): Int {
                return when(concatAdapter.getItemViewType(position)) {
                    // Show 2 Columns when items are normal products
                    // 1 Means that it will take only cell for each item
                    // Now, our GridLayoutManager is of 2 cells, so each row
                    // will show 2 cells.
                    TYPE_NORMAL_PRODUCT_ITEM -> 1

                    // Show Full Width single item when its footer
                    // 2 means it will take 2 cells to create one full width cell.
                    TYPE_FOOTER_ITEM -> 2

                    else -> 1
                }
            }
        }
        return layoutManager
    }
```

Note that `ProductsAdapter.TYPE_NORMAL_PRODUCT_ITEM` and `TYPE_FOOTER_ITEM`. These are simple constant values to let `ConcatAdapter` know that which item currenly being shown. You can have sections, headers, etc. But since I have two adapters, so I defined `TYPE_NORMAL_PRODUCT_ITEM` for my normal items for the Grid, and `TYPE_FOOTER_ITEM` for the Footer. I have to override `getItemViewType()` method for both Adapter classes and return this type like this:

```kotlin
// ProductsAdapter class
override fun getItemViewType(position: Int): Int {
    return TYPE_NORMAL_PRODUCT_ITEM
}

// FooterAdapter class
override fun getItemViewType(position: Int): Int {
    return TYPE_FOOTER_ITEM
}
```

Then I attached the customized `GridLayoutManager` to my `RecyclerView` like this:

```kotlin
val layoutManager = getFooterAdjustedGridLayoutManager(requireContext())
binding.recyclerView.layoutManager = layoutManager
```

**Remember that if I don't add extra item in the grid to make total items count an even number, then the app crashes.**

**References**
* https://stackoverflow.com/questions/39303632/set-last-grid-to-full-width-using-gridlayoutmanager-recyclerview/39304170
* https://medium.com/android-dev-journal/how-to-make-first-item-of-recyclerview-of-full-width-with-a-gridlayoutmanager-66456a4bfffe
* A talk about Mastering RecyclerViews https://www.youtube.com/watch?v=-C5I1DAviJ8
* https://stackoverflow.com/questions/30889066/recycleviews-span-size
* A very good generic OnScrollListener for RecyclerViews https://gist.github.com/pratikbutani/dc6b963aa12200b3ad88aecd0d103872#gistcomment-3338185
* https://oozou.com/blog/implementing-a-nested-recyclerview-in-android-with-concatadapter-57
* https://developersbreach.com/merge-multiple-adapters-with-concatadapter-android/
* https://blog.mindorks.com/implementing-merge-adapter-in-android-tutorial
* https://www.valueof.io/blog/recyclerview-concatadapter-view-types
