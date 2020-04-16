# The Modern Cpp Challenge on Mobile - Sexy prime pairs

Hello ! I'm Xavier Jouvenot and here is the fifth part of a long series on [The Modern C++ Challenge](https://amzn.to/2QdYmvA).
In this article, I am going to explain how I solved the fifth problem in C++, and how I integrated the solution in an Android project.

The objective of this fifth problem is simple.
We must that prints all sexy prime pairs up to a limit entered by the user.
The solution will be computed in C++ and the interface to get the user input and display the result will be handled with the Android Studio Framework.

I encourage you to read the [previous part of this series](https://10xlearner.com/2020/04/15/the-modern-cpp-challenge-on-mobile-largest-prime-smaller-than-given-number/), since we are going to continue our program created in it.

## Sexy prime pairs, what are they 🤔

The property of a sexy prime pair is very simple.
This is a pair of prime numbers that differ from each other by 6.

Since we have seen in the previous part how to identify a prime number, this problem will be easy to solve.

## The C++ solution

As I said, in the [previous part](https://10xlearner.com/2020/04/15/the-modern-cpp-challenge-on-mobile-largest-prime-smaller-than-given-number/), we have implement have been able to identify prime numbers, so, in this solution, we are going to reuse the function `bool isPrime(unsigned int number)` implemented before.
reuse `isPrime` from the previous problem.

To do so, we are going to extract this function is a different file and link it as a static library to the previous problem and to our current problem solution, in our `CmakeLists.txt` file.

```cmake
add_library(primeNumber STATIC primeNumber.hpp)
set_property(TARGET primeNumber PROPERTY LINKER_LANGUAGE CXX)

target_link_libraries(problem4 PRIVATE primeNumber)
target_link_libraries(problem5 PRIVATE primeNumber)
```

Now that we can access the `isPrime` function, we can use it to create a method able to identify a sexy prime pair:

```cpp
#include <cmath>

static const bool isSexyPrimePair(unsigned int firstNumber, unsigned int secondNumber)
{
    return (std::abs(static_cast<int>(firstNumber - secondNumber)) == 6) && isPrime(firstNumber) && isPrime(secondNumber);
}
```

In this function, we check if the two numbers are prime numbers, and, using `std::abs`, we check if the difference between the 2 of them is `6`.

I didn't make this function `constexpr` because `std::abs` is not `constexpr`.
I was wondering why, so I ask a question on [reddit](https://www.reddit.com/r/cpp_questions/comments/g2j7d8/stdabs_and_constexpr/). hopefully, by the time you will be reading this, this answer will have an answer, or if you know it, please leave a comment so that everyone will know why 😉

You can note that I could have workaround `std::abs` to make this function `constexpr`, but I want to use the `std` as much as possible (just a personal wish 😉)

Now that we have a function able to identify a sexy prime pair, we can implement a function to solve completely our problem.

```cpp
std::vector<std::pair<unsigned int, unsigned int>> sexyPrimeSmallerThan (const unsigned int upperLimit)
{
    std::vector<std::pair<unsigned int, unsigned int>>  results;
    for(unsigned int i = upperLimit; i > 6; --i)
    {
        auto firstIndex = i;
        auto secondIndex = static_cast<unsigned int>(i-6);
        if(isSexyPrimePair(i, secondIndex))
        {
            results.emplace_back(std::make_pair<unsigned int, unsigned int>(std::move(firstIndex), std::move(secondIndex)));
        }
    }
    return results;
}
```

In this function, we start from the upper limit given by the user and go down to `6`, and at each number, we check is the current number and the number smaller than it by `6` can create a sexy prime pair. If this is the case, we store this pair in the result data structure before going to the next number.

And voilà, we have a C++ solution able to give us the list of sexy prime pairs we are looking for.

## The UI interface on Android Studio

The user interface for this problem is like the one of of the [previous problem](https://10xlearner.com/2020/04/15/the-modern-cpp-challenge-on-mobile-largest-prime-smaller-than-given-number/). So I won't go into more detail, but I encourage you to go look to my [blog post about it](https://10xlearner.com/2020/04/15/the-modern-cpp-challenge-on-mobile-largest-prime-smaller-than-given-number/). 😉

## Using C++ native code

To link the user interface and the c++ method, we need to transform the result given by the C++ function into something we can print to the user.
To do so, I choose to give to the user interface a string directly generated in the C++ part of the code:

```cpp
extern "C"
JNIEXPORT jstring JNICALL
Java_com_example_themoderncppchallenge_Problem_15_SexyPrimeSmallerThan(JNIEnv *env, jobject thiz,
                                                                       jint user_input) {
    auto result = sexyPrimeSmallerThan(user_input);
    std::string text;
    for(const auto& sexyPair : result) {
        text += '(' + std::to_string(sexyPair.first) + ", " + std::to_string(sexyPair.second) + "), ";
    }
    // Removes the last ", "
    text.pop_back();
    text.pop_back();
    return env->NewStringUTF(text.c_str());
}
```

In this code, we format the code manually to generate a string that the user interface will print like the result of the previous problem.

## Conclusion

Voilà ! We now have an application which can solve the first four problems of [The Modern C++ Challenge](https://amzn.to/2QdYmvA).

You can note that the solutions, written in this post, don't include all the sources to make a running program, but only the interesting part of the sources to solve this problem.
If you want to see the programs from end to end, you can go on my [GitHub account](https://github.com/Xav83/TheModernCppChallenge_AndroidStudio/tree/v0.0.5_FifthProblem), explore the full solution, add comments or ask questions if you want to, on the platform you read this article, it will also help me improve the quality of my articles.

Thank you all for reading this article,
And until my next article, have a splendid day 😉

![](https://github.com/Xav83/Xav83.github.io/raw/master/res/TheModernCppChallenge/Problem%20005.png "Final Application")

## Interesting links

- [The Modern C++ Challenge](https://amzn.to/2QdYmvA)
- [Github release](https://github.com/Xav83/TheModernCppChallenge_AndroidStudio/tree/v0.0.5_FifthProblem)
- [Sexy prime numbers definition](https://en.wikipedia.org/wiki/Sexy_prime)
- [std::abs and constexpr reddit question](https://www.reddit.com/r/cpp_questions/comments/g2j7d8/stdabs_and_constexpr/)
- [std::abs documentation](https://en.cppreference.com/w/cpp/numeric/math/abs)
- [constexpr documentation](https://en.cppreference.com/w/cpp/language/constexpr)
- [std::pair documentation](https://en.cppreference.com/w/cpp/utility/pair)
- [std::make_pair documentation](https://en.cppreference.com/w/cpp/utility/pair/make_pair)
- [std::string::pop_back documentation](https://en.cppreference.com/w/cpp/string/basic_string/pop_back)
