# Quick Tip - How to have an EditText which takes only numbers ?

Hello ! I'm Xavier Jouvenot and in this small post, I am going to explain how to create an EditText which takes only numbers.

_Self promotion_: You can find other articles on Android development on my [website](www.10xlearner.com) 😉

## Numbers only by default

To make sure that a user only enter numbers in an `EditText`, there are several solution.
And the first one we are going to see is to specify that the `EditText` should only take numbers by default.

To do so, we are going to modify the `EditText` XML definition, and add one attribute to it.
This attribute is `android:inputType` and can be set to a lot of different values depending on what you need. Here is a [link to the documentation]([android:inputType documentation](https://developer.android.com/reference/android/widget/TextView#attr_android:inputType)) if you want to learn more about them.
This attribute is in fact a `TextView` attribute, but since `EditText` inherits from `TextView`, then it works.

Here is how an `EditText` XML definition looks like, when you want to allow only numbers:

```xml
<EditText
    <!--Some attributes-->

    android:inputType="number"

    <!--Some other attributes --> />
```

If not specified, this attribute is set to `text` for an `EditText`.

## Modifying the input type dynamically

Specifying the attribute in the XML is great!
But if you want to modify the style of a `EditText` or a `TextView` while the program is running, then, you must use some Java code and the method `setRawInputType`.
Here is how it looks like:

```java
EditText et = findViewById(R.id.my_edit_text_id);
et.setRawInputType(InputType.TYPE_CLASS_NUMBER);
```

If you want to know what other option you can pass to `setRawInputType`, I encourage you to look at the [InputType documentation](https://developer.android.com/reference/android/text/InputType).

--------------

Thank you all for reading this article,
And until my next article, have an splendid day 😉

## Interesting links

- [android:inputType documentation](https://developer.android.com/reference/android/widget/TextView#attr_android:inputType)
- [setRawInputType documentation](https://developer.android.com/reference/android/widget/TextView#setRawInputType(int))
- [InputType documentation](https://developer.android.com/reference/android/text/InputType)
- [EditText documentation](https://developer.android.com/reference/android/widget/EditText)
