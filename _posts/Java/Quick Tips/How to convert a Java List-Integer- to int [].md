# How to convert a Java List<Integer> to int []

Hello ! I'm Xavier Jouvenot and in this small post, I am going to explain how to convert a Java `List<Integer>` to `int []`.

_Self promotion_: You can find other articles on Android development on my [website](www.10xlearner.com) 😉

## Solution

For the people who only want the solution, to quickly copy-paste it in there code, here it is 😉

```java
List<Integer> my_list = new ArrayList<Integer>(); // create the list
// some code to fill the list...
int [] my_array = my_list.stream().mapToInt(i->i).toArray(); //convert the list into a int[]
```

## Explaintation

If you read this, it may be that you want to understand how the previous solution acheive the goal of transforming the `List<Integer>` into a `int []`, and this is what I am going to try to explain. 🙂 

First of all, we take the filled list and we call the method [stream](https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html#stream--), which is going to convert our `List` into a `Stream`. A `Stream` is "A sequence of elements supporting sequential and parallel aggregate operations.", as definied by the documentation. And with this class, we are one step closer from the type we want.

Then, we call the function [mapToInt](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html#mapToInt-java.util.function.ToIntFunction-) from the `Stream` class. This method returns a `IntStream` containing the result of the operations passed in parameter, and since we are telling it to not change the value of the element (`i` stays `i`), all this function does is to convert the `Stream` into a `IntStream`.

Finally, we call the method [toArray](https://docs.oracle.com/javase/8/docs/api/java/util/stream/IntStream.html#toArray--) of the class `IntStream`. This method convert the `IntStream` to a `int []`, which is exactly what we aimed for. 🙂

--------------

Thank you all for reading this article,
And until my next article, have a splendid day 😉

## Interesting links

- [List::stream documentation](https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html#stream--)
- [Stream::mapToInt documentation](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html#mapToInt-java.util.function.ToIntFunction-)
- [IntStream::toArray documentation](https://docs.oracle.com/javase/8/docs/api/java/util/stream/IntStream.html#toArray--)
- [10xlearner website](www.10xlearner.com)
