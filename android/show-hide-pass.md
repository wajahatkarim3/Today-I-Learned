# Show/Hide Password in EditText in Android

To showing/hiding the password dots in the `EditText` in android, here's the easy one-line way.

Here's the dummy layout with an `EditText` and a `CheckBox` for toggling the password dots.
```xml
<?xml version=”1.0" encoding=”utf-8"?>
<LinearLayout xmlns:android=”http://schemas.android.com/apk/res/android"
  android:layout_width=”match_parent”
  android:layout_height=”match_parent”
  android:layout_margin=”16dp”
  android:orientation=”vertical”>

    <EditText
      android:id=”@+id/edtPassword”
      android:layout_width=”match_parent”
      android:layout_height=”wrap_content”
      android:hint=”Enter password”
      android:inputType=”textPassword” />

    <android.support.v7.widget.AppCompatCheckBox
      android:id=”@+id/checkbox”
      android:layout_width=”wrap_content”
      android:layout_height=”wrap_content”
      android:text=”Show Password” />
      
</LinearLayout> 
```

Now, you can toggle the password on the `OnCheckedChangeListener` using this snippet.

```java
checkbox.setOnCheckedChangeListener(new CompoundButton.OnCheckedChangeListener() {
    @Override
    public void onCheckedChanged(CompoundButton compoundButton, boolean value) {
        if (value)
        {
            // Show Password
            edtPassword.setTransformationMethod(HideReturnsTransformationMethod.getInstance());
        }
        else
        {
            // Hide Password
            edtPassword.setTransformationMethod(PasswordTransformationMethod.getInstance());
        }
    }
});
```
