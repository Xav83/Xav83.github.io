# Quick Tip - How to convert a Java List<Integer> to a Java String

Hello ! I'm Xavier Jouvenot and in this small post, I am going to explain how to convert a Java `List<Integer>` to a Java `String`.

_Self promotion_: You can find other articles on Android development on my [website](www.10xlearner.com) 😉

## Solution

For the people who only want the solution, to quickly copy-paste it in there code, here it is 😉

```java
List<Integer> my_list;
// some code to fill the list ...

StringBuilder sb = new StringBuilder();
for (int i = 0; i < my_list.size(); ++i)
{
    sb.append(my_list.get(i));
    if(i != my_list.size() - 1)
    {
        sb.append(", ");
    }
}
String s = sb.toString();
```

## Explanation

If you read this, it may be that you want to understand how the previous solution achieve the goal of transforming the `List<Integer>` into a `String`, and this is what I am going to try to explain. 🙂 

First of all, we create the list and fill it with whatever integer you want. And we create a [StringBuilder](https://docs.oracle.com/javase/7/docs/api/java/lang/StringBuilder.html) which is going to help us doing the transformation.

Then, we transfer the information from the [List<Integer>](https://docs.oracle.com/javase/8/docs/api/java/util/List.html) to the `StringBuilder` and format it how we want to. In our case, we are separating the each numbers of the list with a comma and a space.

Finally, we create a [String](https://docs.oracle.com/javase/7/docs/api/java/lang/String.html) with all the information we want, by using the method `toString` of `StringBuilder`.

--------------

Thank you all for reading this article,
And until my next article, have a splendid day 😉

## Interesting links

- [StringBuilder documentation](https://docs.oracle.com/javase/7/docs/api/java/lang/StringBuilder.html)
- [List documentation](https://docs.oracle.com/javase/8/docs/api/java/util/List.html)
- [String](https://docs.oracle.com/javase/7/docs/api/java/lang/String.html)
- [10xlearner website](www.10xlearner.com)
