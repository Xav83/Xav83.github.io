# Quick Tip - How to specify a variable element in a resource string ?

Hello ! I'm Xavier Jouvenot and in this small post, I am going to explain how to specify a variable element in a resource string.

_Self promotion_: You can find other articles on Android development on my [website](www.10xlearner.com) 😉

## Strings resources

When crafting you application, you may have encountered some problems when placing your string into the resources.
You may have some text in your application in which you want to be able to modify only some specific elements changing depending on the time, or on user input.

For example, in my application solving [The Modern C++ Challenge](https://amzn.to/2QdYmvA) problems, I have a `TextView` indicating the result of each problem, and the text in this `TextView` can change depending on the user input, but, it always starts with "Result: " followed by the result. You may have similar text where you want to have some variable elements inside a well predefined text.

Hopefully for us, there is an elegant solution available to us : the [formatted strings](https://developer.android.com/guide/topics/resources/string-resource).
Those strings allows us to specify places inside a string where some elements will be added, and which kind of elements it is going to be.

To come back to my example, my string resource declaration with a formatted string looks like that :  `<string name="result">Result: %1$s</string>`
The `%1$s` is the place where my result will be displayed.

Let's explain how to works, so that you can use it too.
In the expression `%1$s`, there are actually two important information:

- The number `1` is the number of the argument passed to fill the formatted string. To specify the value put in place of the expression, you are going to use the function [getString](https://developer.android.com/reference/android/content/res/Resources#getString(int,%2520java.lang.Object...)). The number `1` refers directly to the first argument passed to this function, so that you can have the same argument repeated in several place in your string, and have several arguments passed to your text.

- The final `s` specifies that it will be a string. You can replace it by a `d` to specify that you are going to insert a number instead.

In the end, you can have a line in your resources file looking like:

```xml
<string name="MyText">Hello %1$s %2$s ! My name is %1$s too !</string>
```

--------------

Thank you all for reading this article,
And until my next article, have a splendid day 😉

## Interesting links

- [Formatting strings documentation](https://developer.android.com/guide/topics/resources/string-resource#formatting-strings)
- [getString documentation](https://developer.android.com/reference/android/content/res/Resources#getString(int,%2520java.lang.Object...))
- [10xlearner website](www.10xlearner.com)
