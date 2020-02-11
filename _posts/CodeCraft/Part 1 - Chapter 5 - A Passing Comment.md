# How to write proper comments ? - Code Craft

Hi dear reader, I'm Xavier Jouvenot and this is the fifth article about [Code Craft, by Pete Goodliffe](https://amzn.to/2ZrTaHQ).
If you want, you can look at the previous article about ['Self Documentation'](https://10xlearner.com/2020/01/29/318/), and you can find [book here](https://amzn.to/2ZrTaHQ).

The chapter we will treat today, is named '*A Passing Comment - How to write Code Comments*'.

## Back to the basics - What is a comment ?

A **comment** is an element in a source code which is not read by the compiler/interpreter.
It's only purpose is to be read by humans. This is actually very important for any programmer to understand this correctly.

**COMMENTS ARE MADE TO BE UNDERSTANDABLE AND MAINTAINABLE FOR THE NEXT PROGRAMMER**

This statement means that you should avoid writing too much comments, this is a nightmare to maintain, and most people won't read them. To avoid that, write ['Self Documenting Code'](https://10xlearner.com/2020/01/29/318/), and don't comment code which is self-explanatory.

Of course, there are some tools that can use comments to analyse your code, but this is a little part of the comments that you will write in your code. Most of them are for all the programmers that are going to interact with it, after you have written it, even the future you 😉

## What should I put (and not put) in a comment then ?

To live by the statement, described in the previous part, when writing code, the author of [Code Craft](https://amzn.to/2ZrTaHQ) gives us a list of elements to put or not in the comment.
The motivation behind this list is that "Bad comment are worst than no comment" since they are going to mislead you.
To come up with this list and example of code, the author must have seen some really weird comments 😆

### The wanted ones

Comments should and must explain the **WHY** the code is that way : why this implementation has been chosen when another one could have done the job, or why this code exist, what is its purpose.

Too many times, we see comments trying to explain how the code works when there is nothing more precise than the code to describe what the code does. If you encounter poorly written code, if possible don't comment it, but rewrite it to make it self-explanatory.
> "Don't document bad code - rewrite it", [The Elements of Programming Style by Brian W. Kernighan and P. J. Plauger](https://amzn.to/31L3HyU) 🙂

The other thing you want in your comment is to explain unexpected behavior like workaround and tricks you have to use,
for example, to avoid a bug in a library or in a compiler.

### The unwanted ones

First of all, don't describe the code, use ['Self Documenting Code'](https://10xlearner.com/2020/01/29/318/).
Yes, this rule again, but I can stress enough how important this is.
Moreover, if your code is too complex, and you want to explain what it does with comment, try to refactor it to make the logic appear in the code. It will be much clearer that way.

The second point is that you should not write code in a comment. It will deprecate quickly since nothing will compile or interpret it.
Prefer removing this code and use code versioning if you want to go back to this code.

The third point is to avoid putting restriction, limits or how the code should be used in comments.
Instead try to enforce the behavior using [Defensive programming](https://10xlearner.com/2020/01/06/defensive-programming-code-craft/) or enforce the [limits of your variable](https://10xlearner.com/2020/01/08/how-can-you-check-type-limits-in-c-and-create-your-own-limits-%f0%9f%98%89/) by defining a proper type.

Finally, don't write any jokes, or personal message in your comments.
Personally, I never encountered any of this, but this is really bad for the credibility of the program and is counterproductive.

## How to work with comments

Finally let's see how you can work with comment on some specific points. 🙂

### Comment layout

The layout of your comments should definitely be defined in your [coding standard](https://10xlearner.com/2020/01/15/coding-standard-code-craft/).
Indeed, it is easier to have some template for functions, class or file comments when you are using tools like [Doxygen](http://www.doxygen.nl/) to generate your documentation. It will also make your documentation much more consistent, so it will be easier to understand.

Two other points mentioned by the author, are:
- Comments should be indented along side the code, to make them more coupled with the code they comment
- Avoid [Ascii art](https://www.asciiart.eu/) which are long to maintain.

### Comment as flag

A cool thing with comments is that you can use them to flag some elements in your code.

The author gives three flags that you can use and which are often spotted by IDE, to make you interact more easily with them:
- Comment starting with `XXX` to reference hard to understand code.
- Comment starting with `FIXME` to reference a code with a bug in it.
- Comment starting with `TODO`to reference some implementation not done yet.

The author also mention that you may prefer throwing an exception instead of using the flag `TODO` so that you won't forget to actually implement what is left to do.

Moreover, he also specify that you should avoid referencing bug fix in your code.
Indeed, your code should state the present, what you have, instead of the past.
To do that, you may prefer using a [bug tracker](https://www.softwaretestinghelp.com/popular-bug-tracking-software/) 😉

### Comment to write an algorithm

When you want to implement a difficult algorithm, a technique is a use comments.
To do so, you write a comment for each step of your algorithm (each step can either be in plain English or in pseudo code).
Then, below each step, each comment, you write the code which does exactly what the step is.
Once you have done that, you should have a working algorithm without too much trouble.
All you have left to do is to remove the unnecessary comments. 😉

## Conclusion

Today, we have seen how to use comment.
And more importantly, that our aim should always be to write [self documenting code](https://10xlearner.com/2020/01/29/318/) which doesn't require any comment.

Comments can be a huge advantage when use correctly but can also mislead you or become a nightmare to maintain, so be careful when using them.

Thank you all for reading this article,
And until my next article, have an splendid day 😉
