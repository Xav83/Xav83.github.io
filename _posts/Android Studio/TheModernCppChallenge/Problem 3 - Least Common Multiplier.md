# The Modern Cpp Challenge on Mobile - Least Common Multiplier

Hello ! I'm Xavier Jouvenot and here is the third part of a long series on [The Modern C++ Challenge](https://amzn.to/2QdYmvA).
In this article, I am going to explain how I solved the third problem in C++, and how I integrated the solution in an Android project.

The objective of this third problem is simple.
We must calculate the least common multiplier as manu inputs as the user want to give, and we print it to the user.
The solution will be computed in C++ and the interface to get the user input and display the result will be handled with the Android Studio Framework.

I encourage you to read the [previous part of this series](https://10xlearner.com/2020/03/30/the-modern-cpp-challenge-on-mobile-greatest-commont-divisor/), since we are going to continue our program created in it.

## The C++ solution

To calculate the least common multiplier of several numbers, we can to calculate the least common multiplier of two first numbers, then, calculate the least common multiplier of the previous result with the third number, and continue on until, you used all the numbers to compute the least common multiplier of all the user inputs.
You can note that this solution is naive, there may be ways to improve the performance of this function. 😉

Luckily for us, when looking at the C++17 standard, you can find a [std::lcm](https://en.cppreference.com/w/cpp/numeric/lcm) function which calculates the least common multiplier of two numbers.
So, all we need to do is to use this method to create our own function able to take as many numbers as we need.

```c++
template <class InputIterator>
constexpr auto my_lcm (InputIterator first, InputIterator last)
{
    return std::accumulate(first, last, 1, [](const auto& first, const auto& second){
        return std::lcm(first, second);
    });
}
```

In this function, we use [std::accumulate](https://en.cppreference.com/w/cpp/algorithm/accumulate) to reuse the result of the previous [std::lcm](https://en.cppreference.com/w/cpp/numeric/lcm) calcultation in the lambda function. This function takes two [iterators](https://en.cppreference.com/w/cpp/iterator/iterator) as parameters, to make be able to have as much inputs as we want.
Instead of the iterators, we could have passed a `std::vector` for example.

## The UI interface on Android Studio

Creating a function that can take as many parameters as we want in C++ is one thing that can be easily do.
Giving the ability to the user to do so on his phone, require to think a little but about the user interface.
Indeed, we can display to the user an infinity of `EditText` to meet our requirement. We have to use another solution.

The solution I come up with, requires one EditText, and two buttons : a "Add" button and a "Clear" button.

The `EditText` allows the user to specify his input.

The "Add" button allows him to integrate the number entered in the `EditText` to the least common multiplier calculation.
Each time a new number is added, we compute the least common multiplier with all the numbers added until then and display the result.

The "Clear" button allows the user to reset the least common multiplier by removing all the stored numbers added in memory until now.

With this solution, the user can specify as many input as he wants 🙂

## Using C++ native code

For this problem, I decided to store the user inputs in the Java activity, and send the list of user input to the C++ code to calculate the result.
I could have pass to the C++ each of the inputs as they were entered by the user, but I wanted to experiment with arrays conversion from Java to C++.

In Java, I stored the user input in a `List<Integer>`:
```java
EditText et = findViewById(R.id.input_number);
if(et.getText().length() == 0) { return; }
userInputs.add(Integer.parseInt(et.getText().toString()));
```

This was the easy part.
Indeed, if we want to get an array of integer in the C++ code, we need to convert our `List<Integer>` to a structure called `jintArray`.
And after some googling, I found this one-line code, allowing me to do exactly that:
```java
userInputs.stream().mapToInt(i->i).toArray()
```

This line does a lot of conversion to give use an array of integer.
From `userInputs` which is a `List<Integer>`, to a `Stream` with [stream method]([java Collection::stream documentation](https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html#stream--)). We then convert the `Stream` into an `IntStream` because of the [mapToInt method]([java Stream::mapToInt documentation](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html#mapToInt-java.util.function.ToIntFunction-)). And finally, from the `IntStream` to the array of `int` with the [method toArray]([java IntStream::toArray documentation](https://docs.oracle.com/javase/8/docs/api/java/util/stream/IntStream.html#toArray--)).

But all this is the Java side of the story !
Indeed, we have to get those elements in the C++ now, to be able to feed them to our function.
And this looks something like that:

```c++
extern "C"
JNIEXPORT jstring JNICALL
Java_com_example_themoderncppchallenge_Problem_13_Lcm(JNIEnv *env, jobject /* this */,
                                                      jintArray arr) {
    jsize size = env->GetArrayLength( arr );
    std::vector<jint> input( size );
    env->GetIntArrayRegion( arr, jsize{0}, size, &input[0] );

    auto result = my_lcm(std::begin(input), std::end(input));
    return env->NewStringUTF(std::to_string(result).c_str());
}
```

In this part of the program, we get the `jintArray` (i.e. the array of integer converted from the `List<Integer>`), and convert it to a `std::vector` to be able to give it to our method.
To do so, we need to use some methods from the `JNIEnv` object.

With `env->GetArrayLength( arr )`, we get the size of the array, which we first use to create the `std::vector` with the right size already reserved.
Then, comes the instruction `env->GetIntArrayRegion( arr, jsize{0}, size, &input[0] );` which allows us to copy a region of the array into the native buffer, our `std::vector`.

All we have to do after that, is to call our method, and return the result to display it in our application 🙂

## Conclusion

So, we now have an application which can solve the first three problems of [The Modern C++ Challenge](https://amzn.to/2QdYmvA).

You can note that the solutions, written in this post, don't include all the sources to make a running program, but only the interesting part of the sources to solve this problem.
If you want to see the programs from end to end, you can go on my [GitHub account](https://github.com/Xav83/TheModernCppChallenge_AndroidStudio/tree/v0.0.3_ThirdProblem), explore the full solution, add comments or ask questions if you want to, on the platform you read this article, it will also help me improve the quality of my articles.

Thank you all for reading this article,
And until my next article, have a splendid day 😉

![](https://github.com/Xav83/Xav83.github.io/raw/master/res/TheModernCppChallenge/Problem%20003.png "Final Application")

## Interesting links

- [The Modern C++ Challenge](https://amzn.to/2QdYmvA)
- [Github release](https://github.com/Xav83/TheModernCppChallenge_AndroidStudio/tree/v0.0.2_SecondProblem)
- [std::accumulate documentation](https://en.cppreference.com/w/cpp/algorithm/accumulate)
- [std::lcm documentation](https://en.cppreference.com/w/cpp/numeric/lcm)
- [java Collection::stream documentaion](https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html#stream--)
- [java Stream::mapToInt documentaion](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html#mapToInt-java.util.function.ToIntFunction-)
- [java IntStream::toArray documentaion](https://docs.oracle.com/javase/8/docs/api/java/util/stream/IntStream.html#toArray--)
- [JNIEnv GetArrayLength documentation](http://jikesrvm.sourceforge.net/apidocs/latest/org/jikesrvm/jni/JNIFunctions.html#GetArrayLength(org.jikesrvm.jni.JNIEnvironment,%20int))
- [JNIEnv GetIntArrayRegion documentation](http://jikesrvm.sourceforge.net/apidocs/latest/org/jikesrvm/jni/JNIFunctions.html#GetIntArrayRegion(org.jikesrvm.jni.JNIEnvironment,%20int,%20int,%20int,%20org.vmmagic.unboxed.Address))
