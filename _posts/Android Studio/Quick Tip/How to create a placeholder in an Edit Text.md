# Quick Tip - How to create a placeholder in an EditText ?

Hello ! I'm Xavier Jouvenot and in this small post, I am going to explain how to create a placeholder in an EditText.

_Self promotion_: You can find other articles on Android development on my [website](www.10xlearner.com) 😉

## Specifying the placeholder by default

To add a placeholder to an `EditText` text, there are several solutions.
And the first one we are going to specify a default placeholder for an `EditText`.

To do so, we are going to modify the `EditText` XML definition, and add one attribute to it.
This attribute is `android::hint` and you can specify the text you want to be shown as a placeholder to this attribute.

Here is how such `EditText` XML definition looks like:

```xml
<EditText
    <!--Some attributes-->

    android:hint="A wonderful placeholder."

    <!--Some other attributes --> />
```

If not specified, this attribute is set to an empty string.

## Modifying the placeholder dynamically

Specifying the attribute in the XML is great!
But if you want to modify the text displayed as a placeholder of a `EditText` while the program is running, then, you must use some Java code and the method `setHint`.
Here is how it looks like:

```java
EditText et = findViewById(R.id.my_edit_text_id);
et.setHint(R.string.hello);
```

Since the method `setHint` takes an integer as a parameter, you should definitely use the resources from your application to store the text of the placeholder.

--------------

Thank you all for reading this article,
And until my next article, have a splendid day 😉

## Interesting links

- [android::hint documentation](https://developer.android.com/reference/android/widget/TextView#attr_android:hint)
- [setHint documentation](https://developer.android.com/reference/android/widget/TextView#setHint(int))
- [App resources documentation](https://developer.android.com/guide/topics/resources/providing-resources#java)
