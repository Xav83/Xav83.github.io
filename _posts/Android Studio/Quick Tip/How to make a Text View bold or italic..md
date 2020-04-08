# Quick Tip - How to make a TextView bold or italic ?

Hello ! I'm Xavier Jouvenot and in this small post, I am going to explain how to change `TextView` into a bold or italic.

_Self promotion_: You can find other articles on Android development on my [website](www.10xlearner.com) 😉

## Bold or Italic by default

To transform a `TextView` text into a bold or an italic text, there are several solutions.
And the first one we are going to see is how to make it bold or italic by default.

To do so, we are going to modify the `TextView` XML definition, and add one attribute to it.
This attribute is `android:textStyle` and can be set to `bold`, `italic`, `normal` or `bold|italic`, in case you want you text to be both bold and italic.
Here is how it look like:

```xml
<TextView
    <!--Some attributes-->

    android:textStyle="bold"

    <!--Some other attributes --> />
```

If not specified, this attribute is set to `normal`.

## Modify the text style dynamically

Specifying the attribute in the XML is great!
But if you want to modify the style of a `TextView` while the program is running, then, you must use some Java code and the method `setTypeface`.
Here is how it looks like:

```java
TextView tv = findViewById(R.id.my_text_view_id);
tv.setTypeface(tv.getTypeface(), Typeface.ITALIC);
```

In this sample of code, you can replace `ITALIC` by `BOLD`, `NORMAL` or `BOLD_ITALIC` depending on what you want to do with your `TextView`.

--------------

Thank you all for reading this article,
And until my next article, have a splendid day 😉

## Interesting links

- [android:textStyle documentation](https://developer.android.com/reference/android/widget/TextView#attr_android:textStyle)
- [setTypeface documentation](https://developer.android.com/reference/android/widget/TextView#setTypeface(android.graphics.Typeface,%20int))
- [Typeface documentation](https://developer.android.com/reference/android/graphics/Typeface)
- [10xlearner website](www.10xlearner.com)
