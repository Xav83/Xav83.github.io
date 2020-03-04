# How to check every return statement in c++ ?

Hi dear reader, I'm Xavier Jouvenot and in this article, we are going to talk about how to be sure to check every return statement in C++. This post was inspired by a rule from the first chapter of [Code Craft, by Pete Goodliffe](https://amzn.to/2ZrTaHQ), on [Defensive programming](https://10xlearner.com/2020/01/06/defensive-programming-code-craft/)

## Does it even really matter ?

Actually, it does ! It **really** does ! Let's see why ! 😉

When you write code, you know which functions return something, and which one doesn't in your code.
But when you start to interact with other's people code (which can be code from your past self 😉), things start to become a little bit more blurry. 😝

Indeed, if the person used well-named function, you can probably guess which one return something, and which one doesn't.
For example, a function named `hasSucceed` probably return an element, you can even assume that it is going to return a Boolean or an error code, this is the power of [self-documenting code](https://10xlearner.com/2020/01/29/318/). A quick look in to the prototype of the function with your favorite IDE, and know, you know what to expect about the returned element from this function.

But this is the best case scenario.
In fact, even if you are conscientious enough to look at the definition of each method you see (which is going to take you a long time).
You can't be sure that all of your colleagues are going to do the same.
Even you and I, willing to integrate [Defensive programming](https://10xlearner.com/2020/01/06/defensive-programming-code-craft/), in a bad day, will probably just assume what the function does only by looking at its name and use it. We are only human after all, and we make mistakes. 😉

An even worst case would be if you had to debug some piece of code completely foreign to you.
You can't spend several weeks learning everything there is to know about the library, you have to be able to interact with it quickly and the more safely possible.

And to do so, we need to be able to know when a return statement isn't check by anything.
Disclaimer: Even if you do check all your return statement automatically, it doesn't mean that you are going to magically understand all the code you interact with. This is only a tool that will help you avoid error and make your code safer. 😉

## Ok, so how do I do that ?

In C++, there are several two ways to make sure that the return statement of a function be used: compilers warnings and the attribute `nodiscard`.

### Compiler warnings

All the main compiler (gcc, clang, msvc) have integrated a warning able to check when some function call aren't used.

For example, in [gcc](https://gcc.gnu.org/onlinedocs/gcc/Warning-Options.html) and [clang](https://clang.llvm.org/docs/DiagnosticsReference.html#wunused-result), the name of this warning is `Wunused-result`. In msvc, this is the warning [C6031](https://docs.microsoft.com/en-us/visualstudio/code-quality/c6031?view=vs-2019).

Moreover, this warning is set by default in those compiler 😃
And if you have to, you can always disable this warning locally. There is a great article about that on [Fluent.cpp](https://www.fluentcpp.com/2019/08/30/how-to-disable-a-warning-in-cpp/), you should definitely check it out ! 😉

### The attribute `nodiscard`

In C++17, a new attribute, named [nodiscard](https://en.cppreference.com/w/cpp/language/attributes/nodiscard), has been introduce.

```c++
[[nodiscard]] int getValue() const;
```

And this attribute is great when you want to make sure that the returned variable of one of your method is always used.
Moreover, you can specify a class or an enum with this attribute to force it to be initialized using explicit initialization or `static_cast`.

Either way, this is a good way to make your code more expressive about which function return an element and when this element have to be used.
I really like this attribute 😃


## Conclusion

When you want to write quality software, you have to make sure that you don't forget information given to you, and to make sure that the information you give to other will be used.
And this is exactly what we have seen with this article. Even if the solution given are specific to C++, this concept, this "rule" can be apply in many languages to make better programs, less error-prone. 😉


Thank you all for reading this article,
And until my next article, have an splendid day 😉
