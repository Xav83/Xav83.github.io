# Code Craft - On the Defensive - Readthrough [Part 1 - Chapter 1]

Hi dear reader, I'm Xavier Jouvenot and this is the first article about [Code Craft, by Pete Goodliffe](https://amzn.to/2ZrTaHQ).

This article is the first of a series in which I will go through this book.
It will make me improve as a software developer by resuming it to you, and make you learn about the subject treated in this book, so this is a win-win.
And we can even make a [win-win-win](https://youtu.be/AvwUb35v6JA), for the author, if you get this [book](https://amzn.to/2ZrTaHQ).

The chapter we will treat today, is named '*On the Defensive - Defensive Programming Techniques for Robust Code*'.

## Defensive programming, what is it ?

If you go on [Wikipedia](https://en.wikipedia.org/wiki/Defensive_programming), you can have a definition such as:
> Defensive programming is a form of defensive design intended to ensure the continuing function of a piece of software under unforeseen circumstances.

So basically, you plan for the worst scenarios, and it makes sense.
Indeed, your code will be used in unexpected manners: for example, you or some collaborators may make some mistakes in the code call, or a "evil hacker" can try to make your code do something unintended.
Whatever the case, you better guard against such thing, and remember [Murphy's Law](https://en.wikipedia.org/wiki/Murphy%27s_law) '**If it can happen, it will happen**'.

Moreover, the author even says that this is on of the big difference between a Good Code and a Correct code.
He defines "Good Code" some code that does what the writer (or the writers) of the code intended *all the time*, whereas "Correct code" is simply a code which has a valid syntax and work most of the time.

So defensive programming can make you write good code. Sounds appealing, right ? 😉

But first, let's look at the Pros and Cons of defensive programming.

**Pros**:

- You save a lot of your debugging time! Indeed, when you work with good code, you are rapidly notified if you are using it badly, which will help you debug your program much faster than if the code give you no clue.

- Your code will be more persistent, more robust through time. Once written, good code will have less chance to find a bug in it, or to rewrite it in case of modification in the specifications of the software. 💪

**Cons**:

- You will have to do more work upfront. Indeed, writing good code require that you spend more time than writing only correct code. But this is time that you, or someone else won't spend on debugging the code you've written 😉

- The code will consume more resources. And this is something than can be huge depending on your project. Indeed, one check call one time won't be much, but several checks call many thousands of times, or even more, can really slow your program. There are some techniques that can be used to minimize that problem (for example, removing automatically the checks when creating a release, and nor during the debug), so don't worry too much about this one.

To conclude this Pros and Cons, we can say that we are better with Defensive Programming than without it. 😄

## General Techniques/Rules for Defensive Programming

Now that we know what defensive programming is, let's take a look at how we can integrate it into our developer's lives.
The author list some rules to achieve it, and we are going to take a look and comment at some of them.

- [Use Good Coding Style](https://10xlearner.com/2019/12/05/formatting-cpp-c-javascript-and-other-stuff/)
- Take your time
- [Write Readable Code](https://code.tutsplus.com/tutorials/top-15-best-practices-for-writing-super-readable-code--net-8118)
- [Create Good Architecture](https://sourcemaking.com/)
- Trust No One
- [Use the Encapsulation](https://en.wikipedia.org/wiki/Encapsulation_(computer_programming))
- [Use Compiler Warnings](https://www.viva64.com/en/t/0080/)
- [Use Static Analysis Tools](https://www.viva64.com/en/t/0046/)
- Use Safe Data Structure
- Check Every Return Value
- Be Careful with Memory Management
- [Avoid Undefined Behavior](https://en.wikipedia.org/wiki/Undefined_behavior)
- [Rely on Language Specification](https://en.wikipedia.org/wiki/Specification_language)
- Use Good Logging
- Cast Carefully
- Specify Default Behavior
- Check Numeric Limits
- [Use Almost Always Const](https://en.wikipedia.org/wiki/Const_(computer_programming)#Loopholes_to_const-correctness)

## Guard against what ? and where ?

After reading the rules given by the author, you probably have a some elements of *what* we should guard against, when using defensive programming: numeric limits, undefined behavior, for example. But the list of things that we should guard is much more longer.
Indeed, bad inputs, [sanity checks](https://en.wikipedia.org/wiki/Sanity_check) on function result, places in the code that the program should not reach, are some other examples of things we should be defensive about, but this list is not exhaustive, and you can comment if you have any other example 😉

The other question that you may have is : Where should I place the guards in my code.
The author defined 4 scenarios, "places" in the code where you should have guards:
- Pre-condition, which are the conditions that must always be true at the *beginning* of a function, before the code of the function, like input validity.
- Post-condition, which are the condition that must always be true at the *end* of a function, after all the code of the function, to guard against error in the function code.
- Invariant, which are conditions that are always true at *some point in the program execution*, and which validate the program logic
- Assertion, which are all the other check of the program state at any given point in time.

You probably do some or all of those checks, even if you did not know it, but having a name for them is really important.
Indeed it will allow to communicate and talk about them with your coworker more easily and more clearly, which is really important when you want your team to be defensive about the code that each of you write.

## Offensive programming

Before we finish on the last part of the chapter, the author mention the concept of [*Offensive Programming*](https://en.wikipedia.org/wiki/Offensive_programming).

Unlike the name can suggest, this is not the opposite of the Defensive Programming, but the idea is that we should not have any tolerance for errors in the wrong place. So Offensive programmers will be used more assertions, making the program crash when an error is detected, instead of guarding against it (falling back in a valid state, by preventing the user that an error occurred, for example).

The two approach can and should be used together, by using Offensive programming when we can write tests to validate that the assertion should never be trigger, or only when a developer makes a mistake, on use Defensive programming to avoid any problem that can be triggered by external elements, such as the operating system or the user.

## Conclusion

Defensive programming is a really good concept and I like that the author starts his book with this concept.

We should all use defensive programming (and offensive programming), and being even a little more defensive about your code will make it safer for everyone.

Thank you all for reading this article,
And until my next article, have a splendid day 😉