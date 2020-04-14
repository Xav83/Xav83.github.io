# Magic numbers and how to deal with them in C++

Hi dear reader, I'm Xavier Jouvenot and in this article, we are going to talk about magic numbers. This post was inspired by a rule from the fourth chapter of [Code Craft, by Pete Goodliffe](https://amzn.to/2ZrTaHQ), on [Defensive programming](https://10xlearner.com/2020/01/29/318/).

## What is a Magic number ?

A magic number is a raw number in the code.
This is as simple as that.
But what the matter with those raw number, may you ask ?
Well, let's talk about them !

Imagine facing some code like this:
```c++
auto result = function(3);
if(result == 42)
{
    otherFunction(3);
    return 42;
}
return -1;
```

What the numbers of this code mean ?
Are the two '42' same information ?
Is the parameter of `otherfunction` `3` because `function` also takes `3` as a parameter ?
When returning -1, is it an error code, or something else ?

Those can be a few of the questions about the numbers that you can ask yourself when looking at such a code.
The main problem of such number is that you have no idea of what they represent, which make the understanding of the code harder for new comers, or for people used to the code base too.

Another problem with such number can be encountered when refactoring your code.
Imagine that the `42` represent an error code, and it is now a valid result (since it's the [Answer to the Ultimate Question of Life, the Universe, and Everything](https://amzn.to/3cydfCC) 😉) and you have to modify it to `43`.
In all you code base, you have `42`s and your task is to modify the right one into `43`, but to let the `42`s where they should not be change in `43`.
This example may sound a bit silly, but refactor of such nature does happen, and, if you aren't prepare, it's going to cost you a great amount of time, energy and money.

## How to deal with Magic numbers ?

The first thing to do, is to name everything.
By that, I mean storing all your numbers into well-named variables. To do so, I encourage you to read this article about [The Power Of Naming](https://10xlearner.com/2020/01/21/the-power-of-naming-code-craft/).

When naming things, you will end up with one variable being used in several places, where you had the same number several time.
After that, the refactor we mentioned will be super easy, as you will only have to modify the value of one variable.

Moreover, the logic of your code will be much clearer, since you will be able to know the difference between to magic number when they don't represent the same thing.
Let's modify the previous code example, and name the magic numbers:
```c++
const auto param = 3;
const auto result_if_success = 42;
const auto result_if_error = -1;
const auto answer_to_the_ultimate_question_of_life_the_universe_and_everything = 42;

auto result = function(param);
if(result == answer_to_the_ultimate_question_of_life_the_universe_and_everything)
{
    otherFunction(param);
    return result_if_success;
}
return result_if_error;
```

And now, even with poorly named function, we can make a distinction between the different meanings of the numbers, which is a huge benefit to all the people who are going to read such code.

After naming everything, you may end up with some variable having some link between each other.
You can also regroup them in an `enum` or a `namespace` to make this link more visible to everyone.
For example, in our code, we can totally imagine having such a structure:
```c++
const auto param = 3;
const auto answer_to_the_ultimate_question_of_life_the_universe_and_everything = 42;

enum class Result
{
    success = 42,
    error = -1
}

auto result = function(param);
if(result == answer_to_the_ultimate_question_of_life_the_universe_and_everything)
{
    otherFunction(param);
    return Result::success;
}
return Result::error;
```

## Numbers but not only - Magic constants

The concept of Magic numbers can be extended to more that numbers.
Indeed, in your code, you may have some elements which can be like raw, hard coded without any variable to give it a meaning with a well chosen name.

You can for example have strings in you code which can represent different elements such as path to a file or to a directory.
You can also have colors represented as string or as hexadecimal value.
Those elements should always be use through well named variable to make them easily changeable and make your code more understandable.

To make a rule easy to remember, you can say that you should name all the constants in your code.
That way, every time you encounter a raw element, you will give it a good name, and improve your code readability 😉

### Modern c++ best practices

In Modern C++, there are ways to handle magic constants properly.

Indeed, since c++11, you can use the `constexpr` keyword to define variable at compile-time, allowing you to reduce the overhead produce by those variables in your program and make the compiler optimize your code better. That way, you can have `constexpr` numbers, `string_view` (since c++17), or any structure if you designed it the right way.

Moreover, to give more context to your constants and avoid naming conflict, you can use `enum class` and `namespace`.

By using those, I can guarantee you that your code will be much clearer, without any lost of performance.
It's an absolute win 😃

## Conclusion

I can state this enough, but naming is a difficult thing to do, but an essential thing to do.
And magic constants are elements you must name to make your code more expressive, and easier to understand and easier to work with.

Thank you all for reading this article,
And until my next article, have a splendid day 😉

## Interesting links

- [The Power Of Naming](https://10xlearner.com/2020/01/21/the-power-of-naming-code-craft/)
- [Magic numbers on Wikipedia](https://en.wikipedia.org/wiki/Magic_number_(programming))
