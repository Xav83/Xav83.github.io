# To const or not to const

Hi dear reader, I'm Xavier Jouvenot and in this article, we are going to talk about const-correctness, about when and why you should use `const` keyword. This post was inspired by a rule from the first chapter of [Code Craft, by Pete Goodliffe](https://amzn.to/2ZrTaHQ), on [Defensive programming](https://10xlearner.com/2020/01/06/defensive-programming-code-craft/).

## The Const-correctness Principle

To define what *const-correctness* is, we must first defined what *const* is. To quote [Wikipedia](https://en.wikipedia.org/wiki/Const_(computer_programming)) on the subject:
> *const* is a type qualifier: a keyword applied to a data type that indicates that the data is read only.

This sums up well what *const* is.
It also can have some specific meaning depending the languages or the type it qualifies, but we will talk about it later.

Now that the definition of *const* is well established, let's take a look at what *const-correctness* is :
> *const-correctness* is the rule which specify that all variables or objects should be declared with the *const* keyword unless they need to be modified.

## Why is it important ?

So ok, we have the definition, but you may wonder why such a rule? Why should we care ? Why is it even mentioned in the first chapter of [Code Craft, by Pete Goodliffe](https://amzn.to/2ZrTaHQ) ? 🤔

First of all, using *const* whenever we can will make our code easier to understand, and to reason about.
Indeed, if you know that a variable is qualified with *const*, then, you don't have to think that it may have been modified.
It can also allow you to specify what are the input of a function by forcing them to be Read-only, and just like that, you gives an information about the value's intended use.

This may not seems a lot, but just like that, this rule will increases the readability and comprehensibility of code and makes working in teams and maintaining code simpler.
Moreover, this rule can also help the compilers and the optimizers when reasoning about code and optimize it better. 💪

Applying the `const-correctness` is also a good way to start defensive programming in a project. It's simple (only 5 characters to add in some places), and your code will start to make more sense, to keep more of the author intentions.

For all of those reason, you should definitely adopt this rule.

## Examples in C++

In C++, you can put the keyword *const* in a lot of places !

Warning : I don't care about the `east const` VS `west const` debate. I find it childish to be honest 😝, but you can talk about it in the comment, if you really want to.

So, I was saying that in C++, you can put the keyword *const* in a lot of places !
And I won't talk about them all, there are actually too many case.
Instead, I will focus on the meaning that the *const* keyword can give, the intent that you can give to an element with it.
But if you want to know it all, I encourage you to look [on the ISO cpp website](https://isocpp.org/wiki/faq/const-correctness#overview-const).

The first case I want to talk about with you is when *const* qualifies a variable.
Depending on the purpose of the variable, the *const* keyword can reflect different intent from the author. 🙂

For example, if an attribute of a class has the keyword *const*, then it means that the author want it to be either set only by a constructor are always the same for all the instance of a class, in which case, he may prefer using the keywords [static](https://en.cppreference.com/w/cpp/language/static) and [constexpr](https://en.cppreference.com/w/cpp/language/constexpr), but the use of `constexpr` can be another article all by itself, so we will only mention it here. 😉

```c++
class MyClass
{
public:
    MyClass(int value) : my_constant(value) {}

private:
    const int my_constant{0};
    static constexpr int my_other_constant{42};
}
```

Another case could be to get the return variable of a function/method. In that case, the *const* will make sure that the result of the function/method won't be accidentally modify, which can be handy for if the content of the variable is an error code, for example. 👍

```c++
const auto error_code = methodReturningAnError();
```

The final example on the variables that we are going to address is when you combine the *const* keyword to the `&` for either a parameter of a function/method, or when getting the result of a function/method, which allows the compiler to avoid copy and to make sure that the variable won't be modified:
```c++
const auto& variable = methodReturningAnHugeElement();
```
Since this "trick" allows you to avoid unnecessary copy, it can be really interesting to use it whenever you can so that your program will be more understandable, faster and your intent clearer for any other programmer that is going to read your code in the future. 💪

And finally, to finish the examples on C++, I want to talk about the use of the *const* keyword in the method prototype.
It allows the method to specifies that it won't modify any attribute of the class and that all the function called in it will do the same.
It may not seems much, but it is a huge indication for the next programmer that is going to read this code, and a be guard for any programmer that is going to modify the code of this function or of any function called by it.

```c++
auto myMethod (auto parameter) const;
```

And voilà, this is it for the intent of the *const* keyword that I wanted to talk about in C++.
If you have any other, please tell me in the comments 😉

## Conclusion

*const-correctness* is a simple but powerful rule that you should definitely use, whatever the language you are using (if it defined a *const* keyword). It will help you add some defense to your code, and clarify your intention for the next developer to pass. And finally, it may even help compilers and optimizers do a better job.

Surely you will have a code with a lot of *const* all over the place, but it will only reflect the logic of your code and the intention of developers try to communicate to you their intention to have a safe and persistent code.

Thank you for reading this article,
And until my next article, have an splendid day 😉

## Extra resources

- [Defensive programming](https://10xlearner.com/2020/01/06/defensive-programming-code-craft/)
- [Const-correctness, by Arne Mertz](https://arne-mertz.de/2016/07/const-correctness/)
- [Const-correctness, by Alex Allain](https://www.cprogramming.com/tutorial/const_correctness.html)
- [Const-correctness documentaiotn on ISOcpp](https://isocpp.org/wiki/faq/const-correctness#overview-const)