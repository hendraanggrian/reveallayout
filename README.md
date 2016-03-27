CircularRevealLayout
====================

Circular reveal animation as easy as:

```xml
<io.github.hendraanggrian.circularreveallayout.view.RevealParentLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <io.github.hendraanggrian.circularreveallayout.view.RevealFrameLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:duration_exit="250"
        app:duration_open="500"
        app:location="bottom_right">

        ...
        
    </io.github.hendraanggrian.circularreveallayout.view.RevealFrameLayout>

</io.github.hendraanggrian.circularreveallayout.view.RevealParentLayout>
```

![CircularRevealLayout Sample](https://raw.github.com/hendraanggrian/CircularRevealLayout/master/CircularRevealLayout.gif)


Dependencies
------------

ozodrukh's <a href="https://github.com/ozodrukh/CircularReveal">CircularReveal<a/>


Requirements
------------

Minimum SDK level of API 15 (4.0+). However as of this writing, the animation will only occur on API 21 (5.0+). When implemented in API below 21, normal activity transition will occur.


Importing
---------

Gradle line `'io.github.hendraanggrian:circular-reveal-layout:0.1.2'`


Activity Usage
--------------

Use `RevealParentLayout` as parent layout.
Inside, use either `RevealFrameLayout`, `RevealLinearLayout`, or `RevealRelativeLayout`.
Inside, put the actual content.

```xml
<io.github.hendraanggrian.circularreveallayout.view.RevealParentLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <io.github.hendraanggrian.circularreveallayout.view.RevealFrameLayout
        android:id="@+id/revealLayout"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:reveal_exit_duration="250"
        app:reveal_open_duration="500"
        app:type="activity">

        ...

    </io.github.hendraanggrian.circularreveallayout.view.RevealFrameLayout>

</io.github.hendraanggrian.circularreveallayout.view.RevealParentLayout>
```

Calling it

```java
RevealFrameLayout revealLayout = (RevealFrameLayout) findViewById(R.id.revealLayout);
revealLayout.setLocation(X, Y);
```

You can get these X and Y points from `OnTouchListener` from previous activity

```java
View mView = findViewById(R.id.view);
Bundle bundle = new Bundle();

mView.setOnTouchListener(new View.OnTouchListener() {
    @Override
    public boolean onTouch(View view, MotionEvent event) {
        bundle.putInt("EXTRA_X", (int) event.getRawX());
        bundle.putInt("EXTRA_Y", (int) event.getRawY());
        return false;
    }
});
        
mView.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View view) {
        Intent intent = new Intent(context, MyActivity.class);
        intent.putExtras(bundle);
        startActivity(intent);
    }
});
```

To trigger animation on exit, override `onOptionsItemSelected()` and `onBackPressed()`

```java
@Override
public boolean onOptionsItemSelected(MenuItem item) {
    switch (item.getItemId()) {
        case android.R.id.home:
            onBackPressed();
            return true;
        default:
            return super.onOptionsItemSelected(item);
    }
}

@Override
public void onBackPressed() {
    revealLayout.animateExit(this);
}
```

#####Styling

To animate the ActionBar, put toolbar view in your layout, then apply this styling

```xml
<style name="MyTheme" parent="Theme.AppCompat.Light.NoActionBar">
    <!-- your styling -->

    <item name="android:windowIsTranslucent">true</item>
    <item name="android:windowBackground">@android:color/transparent</item>
</style>
```


Dialog Usage
--------------------------

Use `RevealParentLayout` as parent layout of your dialog.
Inside, use either `RevealFrameLayout`, `RevealLinearLayout`, or `RevealRelativeLayout`.
Inside, put the actual content.

```xml
<?xml version="1.0" encoding="utf-8"?>
<io.github.hendraanggrian.circularreveallayout.view.RevealParentLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="300dp"
    android:layout_height="150dp">

    <io.github.hendraanggrian.circularreveallayout.view.RevealLinearLayout
        android:id="@+id/revealLayout"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="vertical"
        app:reveal_exit_duration="250"
        app:reveal_open_duration="500"
        app:type="dialog">

        ...

    </io.github.hendraanggrian.circularreveallayout.view.RevealLinearLayout>

</io.github.hendraanggrian.circularreveallayout.view.RevealParentLayout>
```

Calling it

```java
int x = 0;
int y = 0;

// get the x and y point from OnTouchListener
mView.setOnTouchListener(new View.OnTouchListener() {
    @Override
    public boolean onTouch(View view, MotionEvent event) {
        x = (int) event.getRawX();
        y = (int) event.getRawY();
        return false;
    }
});

// then use them as dialog starting point
mView.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View view) {

        final Dialog dialog = new Dialog(rootView.getContext(), R.style.DialogTheme);
        dialog.setContentView(...);

        RevealLinearLayout revealLayout = (RevealLinearLayout) dialog.findViewById(R.id.revealLayout);
        revealLayout.setLocation(x, y);

        dialog.show();
    }
});
```

To animate on dialog exit, use override dialog `cancel()`

```java
final Dialog dialog = new Dialog(rootView.getContext(), R.style.DialogTheme) {
    @Override
    public void cancel() {
        revealLayout.animateExit(this);
};
```

#####Styling

Don't forget to add this styling

```xml
<style name="DialogTheme" parent="Theme.AppCompat.Dialog">
    <item name="android:windowIsTranslucent">true</item>
    <item name="android:windowBackground">@android:color/transparent</item>
</style>
```


License
--------

    The MIT License (MIT)

    Copyright (c) 2015 Hendra Anggrianto Wijaya
    
    Permission is hereby granted, free of charge, to any person obtaining a copy
    of this software and associated documentation files (the "Software"), to deal
    in the Software without restriction, including without limitation the rights
    to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
    copies of the Software, and to permit persons to whom the Software is
    furnished to do so, subject to the following conditions:
    
    The above copyright notice and this permission notice shall be included in
    all copies or substantial portions of the Software.
    
    THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
    IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
    FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
    AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
    LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
    OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
    THE SOFTWARE.
