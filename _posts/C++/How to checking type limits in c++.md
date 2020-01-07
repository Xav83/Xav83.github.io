# How can you check type limits in C++ ? And create your own limits 😉

Hi dear reader, I'm Xavier Jouvenot and in this article, we are going to talk about how to check type limits in c++. This post was inspired by a rule from the first chapter of [Code Craft, by Pete Goodliffe](https://amzn.to/2ZrTaHQ), on [Defensive programming](https://10xlearner.com/2020/01/06/defensive-programming-code-craft/).

## Why check type limits ?

Let's start by talking about why and when we should check the limit of a type.
The first answer that come to my mind, is *because we can* 😝

But there are concrete usage of it 😉

The most common one that I can think about, is when you want to cast a variable from a type to another one. Most of the time, both of the type will have different limits, so adding an assertion to be sure that the value of the variable is well defined in the destination type, can be a good habit of defensive programming to have.

Another case where you may want to do so, is to avoid undefined behavior, or to use at your advantage the defined behavior of the overflow on some types. So you may want to avoid to have an integer variable overflowing if you increment it too much, since it's an *undefined behavior*, but, on the other hand, you may want to have an unsigned variable overflowing on purpose to follow the tic of your CPU when doing embedded programs.

All of those case, involves the limits of the types.
And if you have other examples, I would gladly read them in comments of this article 😄

## C-style limits : the Preprocessor variables

If you have program on C, or on old C++ (or even new C++ poorly not up to date with the standard), you may have encountered those preprocessor variable storing the limits of the std types.

They look like `INT_MIN` and `INT_MAX`, for the integer type, or `FLT_MIN` and `FLT_MAX` for the float type. There are also other constant like `FLT_EPSILON` which represent the difference between 2 float numbers.

You can find the complete list [here](https://en.cppreference.com/w/cpp/types/climits).
Those constants are useful when you need them in C interface, or when coding in C, but if you are using C++, I recommend you to use the next technique 😉

## C++ limits : std::numeric_limits

In the standard C++, there is a class template which provide the limits and some more information of the type of the numerical types of the std. This class is named `numeric_limits`. If you want, you can find it's full documentation [here](https://en.cppreference.com/w/cpp/types/numeric_limits).

It can be used like this:

```c++
#include <limits>

constexpr auto int_upper_limit = std::numeric_limits<int>::max();
```

There are several advantages to this class.

First of all, it provide you a uniform interface for the limits of all the types, which mean that it can be use inside a class template, or function, without a problem, while it will be far more difficult with the C style solution.

```c++
#include <limits>

template <typename T>
class MyClass
{
    void someMethod()
    {
        constexpr auto variable = std::numeric_limits<T>::max();
        // ...
    }
};
```

Moreover, the syntax is far more clearer that the C style solution, and close to the English language. 🤓

Finally, it can be extended to your own types, so that all of the type of your program can use the same interface, and this is what we are going to do now. 😃

## Create our own type limits

As mentioned in the previous part, a huge advantage of `std::numeric_limits` is that it can be used with your own types, so that all the types of your program will have the same interface when looking for limits about them. Here is how you can do it for the maximum:

```c++
#include <limits>

struct my_type;

template<> class std::numeric_limits<my_type>
{
public:
    static constexpr int max() noexcept { return 1; }
};

int main()
{
    constexpr auto my_type_upper_limit = std::numeric_limits<my_type>::max();
    static_assert(my_type_upper_limit == 1, "Error - the limit is not well set");

    return 0;
}
```

Let's look into detail how this example works. 🙂
First, we specify our type, here named `my_type`, to be able to create a [specialization for the class template](https://en.cppreference.com/w/cpp/language/template_specialization) `std::numeric_limits`.

In the specialization of the template class, we create a method `max` which follows the signature defined by the standard. You can, for example, find the prototype of the method `max` [here](https://en.cppreference.com/w/cpp/types/numeric_limits/max), and look at the other methods that can be specified in `std::numeric_limits` [here](https://en.cppreference.com/w/cpp/types/numeric_limits).

Finally, in the `main` function, we call the method `max` using `std::numeric_limits` with our own type. And voilà ! Now we have a type which can use the same interface as the standard for its limits ! 😄

Of course, here we have only specified the `max` method, but you will probably need to specify the `min` and the `lowest` methods too. It can be done as we did for the `max` method. 😉

## Conclusion

I am not here to start a debate on what is better C or C++.

If you are using C, then use the C way to check the limits of your types.
And if you are using C++, then use the C++ way to do so, it will help you to maintain some consistency in the way you check the types limits in your code.

My point is, if you want to integrate defensive programming (and you should), then, you will need to check the limits of your type, and now, you know how to do it in C++. 😄

Thank you all for reading this article,
And until my next article, have an splendid day 😉