# Quick Tip - How to set the visibility of a button

Hello ! I'm Xavier Jouvenot and in this small post, I am going to explain how to set the visibility of a button.

_Self promotion_: You can find other articles on Android development on my [website](www.10xlearner.com) 😉

## Statically

To specify the visibility of a `Button`, there are several solutions
And the first one we are going to specify a default visibility for a `Button`.

To do so, we are going to modify the `Button` XML definition, and add one attribute to it.
This attribute is `android:visibility` and tell if you want the button to be visible are not.
By default, this attribute is set to [visible](https://developer.android.com/reference/android/view/View#VISIBLE).

Here is how such `Button` XML definition looks like:

```xml
<Button
    <!--Some attributes-->

    android:visibility=invisible

    <!--Some other attributes --> />
```

## Dynamically

Specifying the attribute in the XML is great!
But if you want to modify the visibility of a `EditText` while the program is running, then, you must use some Java code and the method `setVisibility`.
Here is how it looks like:

```java
Button button = findViewById(R.id.buttonId);
button.setVisibility(View.INVISIBLE);
```

--------------

Thank you all for reading this article,
And until my next article, have a splendid day 😉

## Interesting links

- [setVisibility documentation](https://developer.android.com/reference/android/view/View#setVisibility(int))
- [Visible variable documentation](https://developer.android.com/reference/android/view/View#VISIBLE)
- [Invisible variable documentation](https://developer.android.com/reference/android/view/View#INVISIBLE)
- [10xlearner website](www.10xlearner.com)
