# Combining Urdu String through Data Binding

Today, I had a case in my app where there was an ```EditText``` and ```TextView```. The goal was that when ```EditText``` is changed, then ```TextView``` value will be updated instantly. So ```TextView``` value will be like this:

```java
    textView = "SOME URDU STRING 1" + editTextValue + "ANOTHER URDU STRING"
```

First, I added a ```String``` value in my ```strings.xml``` file which was like this:

```xml
    <resources>
        <string name="app_name">AppName</string>
        <string name="sarbarah_str">آپ کا گھر کے سربراہ %s سے کیا رشتہ ہے؟</string>        // My String
    </resources>

```

Please note the ```%s``` in the string. That's where magic happens. Then in my layout, I had ```EditText``` and ```TextView``` like this:

```xml
<?xml version="1.0" encoding="utf-8"?>
<layout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools">

    <data>
        <import type="com.wajahatkarim3.myapp.R"/>
    </data>

    <android.support.constraint.ConstraintLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        tools:context=".DemoActivity">


        <EditText
            android:id="@+id/editText"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:onTextChanged='@{(text, start, before, count) -> textView.setText(String.format(context.getString(R.string.sarbarah_str, text)))}' />

        <TextView
            android:id="@+id/textView"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginStart="8dp"
            android:layout_marginTop="8dp"
            android:text="@string/sarbarah_str"
            app:layout_constraintStart_toStartOf="parent"
            app:layout_constraintTop_toBottomOf="@+id/editText" />

    </android.support.constraint.ConstraintLayout>
</layout>
```

So, the main line is:

```xml
    android:onTextChanged='@{(text, start, before, count) -> textView.setText(String.format(context.getString(R.string.sarbarah_str, text)))}'
```

So, whatever text is entered in ```EditText```, that is added in place of ```%s``` in the string, and that value is set to the ```TextView```.

And that's it.
