# Gone Margins in Constraint Layout

When it comes to “GONE” widgets, the library takes margins’ behavior to the next level.

As usual, views marked as “GONE” are not going to be displayed and are not part of the layout itself. Still, ConstrainyLayout has a specific handling for such views.

* Their dimensions will be considered as zero (they will be resolved to a point).
* If they are constrained to other views, they will still be respected, but all its defined margins will become zero.

![](https://cdn-images-1.medium.com/max/1000/1*EG20yDeVnSv6STXWJSq2Rw.jpeg)

This specific behavior (A and B) allows to build layouts where we can mark widgets as “GONE” without breaking the UI. However, we may find ourselves having a different margin value that does not go well with the design.

This is where the gone margin attributes come into play. These allow us to specify another margin that is applied only when a targeted view constraint is gone.

```xml
android:layout_goneMarginStart
android:layout_goneMarginEnd
android:layout_goneMarginLeft
android:layout_goneMarginTop
android:layout_goneMarginRight
android:layout_goneMarginBottom
```

Gone margins can be applied only the constraint’s source view (source side/source anchor point). Not on the widgets we mark as “GONE”.
