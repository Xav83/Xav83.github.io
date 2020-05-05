# Quick Tip - How to find the difference between two numbers

Hello ! I'm Xavier Jouvenot and in this small post, I am going to explain how to find the difference between two numbers.

_Self promotion_: You can find other articles on Android development on my [website](www.10xlearner.com) 😉

## Subtracting number isn't enough ?

First, let's defines precisely what a difference is and why a subtraction isn't enough.

> The difference between two numbers is the absolute delta between them.

To calculate this absolute delta, you have to *subtract* the **smaller** number from the **larger** one.
Indeed, if you subtract the larger one from the smaller one, you won't find the right answer.

Let's take an example:
```
3 - 1 = 2
1 - 3 = -2
```

Here you can see the only when subtracting the smaller number, `1`, from the larger one, `3`, we obtain the right result `2`. 🙂

We can notice, when we inverse the minimum and the maximum, that the result is the opposite number of the absolute delta.

## Solution implementation

Here is a naive implementation for the difference:

```c++
template<class T, std::enable_if_t<std::is_arithmetic_v<T>>...>
auto difference(const T& x, const T& y) noexcept
{
    return std:abs(x - y);
}
```

There are several points that I am going to explain in this solution.

First, let's talk about [std::abs](https://en.cppreference.com/w/cpp/numeric/math/abs).
As I mentioned in the previous part, when you invert the term of the subtraction to get the difference, you get the opposite number of the absolute delta that we want.
Which means that the absolute value of this result is in fact the result that we want.

So using `std::abs`, no matter the min and the max of the parameters, we will always have our absolute delta returned as a result.


Of course, we could have used `std::min` or `std::max`, but the implementation is much clearer with `std::abs`.
Moreover, we are going to talk about `std::min` and `std::max` in the next part. 😉

## Constexpr solution

In the previous part, we have implemented a version of the `difference` non-constexpr when, actually, we can implement one which is.
And the more constexpr things there are, the more happy I am (if you not convince about the utility of `constexpr`, I encourage you to look at the talk [“constexpr ALL the Things!”, by Ben Deane and Jason Turner](https://www.youtube.com/watch?v=PJwd4JLYJJY)).

Here is some implementations that we could think about, by using `std::min` and `std::max`, since both are `constexpr` functions:

```C++
template<class T, std::enable_if_t<std::is_arithmetic_v<T>>...>
constexpr auto differenceUsingMin(const T& x, const T& y) noexcept
{
    return (std::min(x, y) == x) ? (y - x) : (x - y);
}

template<class T, std::enable_if_t<std::is_arithmetic_v<T>>...>
constexpr auto differenceUsingMax(const T& x, const T& y) noexcept
{
    return (std::max(x, y) == y) ? (y - x) : (x - y);
}
```

Why not use `std::min` and `std::max`, but not `std::abs` here, you may wonder ? 🤔
Well, currently, `std::abs` is not a `constexpr` function which make it impossible to use it in this context.
So I ask the [community on Reddit](https://www.reddit.com/r/cpp_questions/comments/g2j7d8/stdabs_and_constexpr/), and for now, nobody was able to tell me why `std::abs` isn't a `constexpr` function when it could be.

Here is an implementation possible:

```C++
template<class T, std::enable_if_t<std::is_arithmetic_v<T>>...>
constexpr auto my_abs(const T& x) noexcept
{
    return x < 0 ? -x : x;
}

template<class T, std::enable_if_t<std::is_arithmetic_v<T>>...>
constexpr auto difference(const T& x, const T& y) noexcept
{
    return my_abs(x - y);
}
```

As you can see, this solution is far more readable than the one using `std::min` and `std::max`.
I hope that we will soon have a `constexpr` implementation of `std::abs` 🙂

--------------

Thank you all for reading this article,
And until my next article, have a splendid day 😉

## Interesting links

- [Difference definition](https://sciencing.com/calculate-delta-between-two-numbers-5893964.html)
- [std::abs documentation](https://en.cppreference.com/w/cpp/numeric/math/abs)
- [std::min documentation](https://en.cppreference.com/w/cpp/algorithm/min)
- [std::max documentation](https://en.cppreference.com/w/cpp/algorithm/max)
- [conditional operator documentation](https://en.cppreference.com/w/cpp/language/operator_other)
- [std::enable_if documentation](https://en.cppreference.com/w/cpp/types/enable_if)
- [std::is_arithmetic_v documentation](https://en.cppreference.com/w/cpp/types/is_arithmetic)
- [std::abs constexpr reddit thread](https://www.reddit.com/r/cpp_questions/comments/g2j7d8/stdabs_and_constexpr/)
- [10xlearner website](www.10xlearner.com)
