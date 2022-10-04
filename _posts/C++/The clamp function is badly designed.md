# The clamp functions are badly designed

Hello ! I'm Xavier Jouvenot and today, we are going to see why the `std::clamp` and `std::ranges::clamp` functions of the C++ standard are badly designed and how we can simply improve it.

_Self promotion_:
Here are a few social networks where you can follow me and check my work as a programmer and a writer 😉
[Personal blog](www.10xlearner.com), [Twitter](https://twitter.com/10xLearner), [GitHub](https://github.com/Xav83/)

## The issue with std::clamp and std::ranges::clamp

Since C++17, the function [std::clamp](https://en.cppreference.com/w/cpp/algorithm/clamp) is available as a helper to keep a variable in a range of value defined by 2 variables specified during the function call. In C++20, the function [std::ranges::clamp](https://en.cppreference.com/w/cpp/algorithm/ranges/clamp) was also added to the C++ standard.

Those function are defined as such:

```c++
namespace std
{
template<class T>
constexpr const T& clamp( const T& v, const T& lo, const T& hi );

template<class T, class Compare>
constexpr const T& clamp( const T& v, const T& lo, const T& hi, Compare comp );

namespace ranges
{
template< class T, class Proj = std::identity,
         std::indirect_strict_weak_order<std::projected<const T*, Proj>> Comp =
             ranges::less >
constexpr const T&
   clamp( const T& v, const T& lo, const T& hi, Comp comp = {}, Proj proj = {} );
}  // namespace ranges
}  // namespace std
```

And code using those function can look like that:
```c++
auto value = std::clamp (variable, 0, 100);
auto other_value = std::ranges::clamp (other_variable, -100, 0);
```

But it also can look like the following code:
```c++
auto value = std::clamp (50, 100, 75);
```

And in that case, it is very difficult to guess what the intent of the developer writing this line of code is.
Indeed, min and max of the range can be reversed because of a mistake in the code, which is, according to the C++ standard, an undefined behavior. Or the developer writing this line thought that the `clamp` function was taking the range before the value to be clamped.

Whatever the origin of the reason, this code doesn't generate any warning during the compilation, when we could easily imagine a solution to make this kind of error not go unnoticed.

## Improvements possible

In order to make the developer's life easier, one solution could be to change the design of the `clamp` function: coupling the 2 parameters related to the range of the `clamp`. We can, for example, couple them using [std::pair](https://en.cppreference.com/w/cpp/utility/pair):

```c++
#include <cassert>
#include <functional>

namespace mystd
{
template<class T, class Compare>
constexpr const T& clamp( const T& v, const std::pair<T, T>& range, Compare comp )
{
   assert(comp(range.first, range.second));
   return comp(v, range.first) ? range.first : comp(range.second, v) ? range.second : v;
}

template<class T>
constexpr const T& clamp( const T& v, const std::pair<T, T>& range )
{
   return clamp(v, range, std::less<T>{});
}
}
```

That way, when calling the function `clamp`, the developer have to make it obvious what the value to clamp and what the range are:

```c++
auto v = mystd::clamp (5, {7, 10});
```

Moreover, an assertion will be triggered if the minimum and maximum of the range are reversed.
```log
/app/example.cpp:10: constexpr const T& mystd::clamp(const T&, const std::pair<_FIter, _FIter>&, Compare) [with T = int; Compare = std::less<int>]: Assertion `comp(range.first, range.second)' failed.
```

We can also make the assertion a little more explicit by using a structure instead of a `std::pair`. Indeed, instead of having `.first` and `.second` elements, we would have named `min` and `max` attributes. That way, the following code:

```c++
namespace mystd2
{
template<class T>
struct clamp_range
{
   T min, max;
};

template<class T, class Compare>
constexpr const T& clamp( const T& v, const clamp_range<T>& range, Compare comp )
{
   assert(comp(range.min, range.max));
   return comp(v, range.min) ? range.min : comp(range.max, v) ? range.max : v;
}

template<class T>
constexpr const T& clamp( const T& v, const clamp_range<T>& range )
{
   return clamp(v, range, std::less<T>{});
}
}

int main()
{
  auto v = mystd2::clamp (5, {10, 7});
}
```

will give us the following assertion:
```log
/app/example.cpp:32: constexpr const T& mystd2::clamp(const T&, const mystd2::clamp_range<T>&, Compare) [with T = int; Compare = std::less<int>]: Assertion `comp(range.min, range.max)' failed.
```

, making it easier for developers to debug the issue, when they encounter it.

## Conclusion

The title of this article may be a little clickbait for some people, and some other will, without a doubt, say that the solutions, described here, can probably be improved (with concepts, for example), and this is probably true! But, the conclusion remains the same, the standard function `std::clamp` can, and should be improved, in order to help developers avoid mistakes that can be avoided.

If you want to experiment with the solution described in this article, I let you a [godbolt link](https://godbolt.org/z/5335n7Wh9), and if you have any idea on how to improve the `std::clamp` function even more, feel free to write a little comment under this article 😉

-----------

Thank you all for reading this article,
And until my next article, have a splendid day 🙂
