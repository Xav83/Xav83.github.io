# Self-documentation

Hi dear reader, I'm Xavier Jouvenot and this is the fourth article about [Code Craft, by Pete Goodliffe](https://amzn.to/2ZrTaHQ).
If you want, you can look at the previous article about ['What's In A Name'](https://10xlearner.com/2020/01/21/the-power-of-naming-code-craft/), and you can find [book here](https://amzn.to/2ZrTaHQ).

The chapter we will treat today, is named '*The Write Stuff - Techniques for Writing "Self-Documenting" Code*'.

## Why is self-documentation ?

As seen in the previous [post](https://10xlearner.com/2020/01/21/the-power-of-naming-code-craft/), correctly naming things is important when you write code. For example, between the instructions `int a = 42;` and `int apple_count = 42;`, the second one explains what the variable is, gives us some information while the first one doesn't. This is an example, on a really small scale, of self-documentation.

In this chapter of the [book](https://amzn.to/2ZrTaHQ), the author focus more on the documentation side of a project, and define "Good Code" as a **well-documented code**. This means that when you write code, you must think about the next developer who will read and/or modify it.

To achieve this purpose, you have to add some documentation. Some projects have an external documentation, some other projects have documentation inside the code (with comments), and some other use self-documentation techniques. The problem with the first two, is that documentation is more work for the developer. Indeed, he has to remember to update it, make sure to produce a documentation in sync with the code, which is really hard, and as all things that are hard, we, as humans, tend not to do it, or do it poorly, which led to out of sync documentation which can mislead people. 😮

Self-documentation counter this default by making the code self explanatory, so that the code becomes its own documentation. This way, you have documentation which is updated automatically when you modify the code, which make it cheaper to maintain, and make the code easier to understand for everyone. 🙂👍

By using self-documenting code, you will also end up with less comment in your code, since there will be less need to explain with comments what the code does. But the comments you will have to write to explain something in your code will now stands out, and alert the developer out the importance of this comment, making important information stands out.

## How to write self-documenting code ?

Now that we have seen the advantages of self-documenting code, and that you are convinced by them (and I hope you are 😄), let's take a look at some techniques given by the author to implement self-documentation into our code:

- *Simple Code With Good Layout*.
    By simple, the author mean to avoid "hard to look code", like code with a lot of nested elements (return early or use **Single Entry Single Exit** code when it's relevant).
    By good layout, the author refers to the use of [coding standard](https://10xlearner.com/2020/01/15/coding-standard-code-craft/), so that your code will have consistent layout through all your project's files.

- *Use meaningful names*.
    This technique is a direct mention of the [previous chapter of his book](https://10xlearner.com/2020/01/21/the-power-of-naming-code-craft/)). 😉

- *Write small atomic functions*.
    As the author said: "One function, one action". By doing so, each function will be easier to understand. Coupled with a good naming, the function will be self-explanatory. And an atomic function will have small to no side-effects, which make it easier to understand too.

- [*Avoid Magic numbers*](https://10xlearner.com/2020/03/06/magic-numbers-and-how-to-deal-with-them-in-c/)
    Magic numbers are raw number in your code. Using well-named variable instead of raw number makes your code far more self-explanatory, by giving your more context instead of a raw number that could be anything.

- *Make important code stands out and easy to read*
    As I explained in the previous part of this article, comments can be used to make important code stands out in a code which use good naming, since there won't be any comment indicating trivial information. Other techniques to make code or interfaces clearer to the other developers can be put in place like the [Pimpl idiom](https://en.cppreference.com/w/cpp/language/pimpl), for example. 😉

- *Group information together*
    This rule can help you to link some part of you code together to gives them more context. You can apply it by using namespaces, files, packages, or module, for example, depending on what is available in your language. 

The author gives other rules, but in my opinion, those are the more important, the one which are going to give you more benefits. 😉

## Existing Methodologies

To end this chapter, the author talks about the some existing methodologies to have a better self-documenting code. Those methodologies should not be integrated in a project if you don't already use the techniques seen before, they are only use to make your code and the documentation even more related one to the other.

The first one is the use of documentation tools.
If you are a Java developer, you have probably interact with one, the built-in Javadoc. If you use other languages, there are also adapted documentation tools such as Doxygen or C#'s NDoc. But what are those tools for if you have already self-documenting code ? 🤔

Actually, they give you the ability to generate a documentation from your source code ! Let's see why this is amazing ! 😉
If you code is self-documenting, those tools can give you the ability to have a document which describe exactly the structure of your code/interfaces, and can allow you to look for some code, see what is available in the project, or allow a new developer to understand more about the project organization.
Moreover, since it's generated from the source code, there will be no risk of mismatch between the documentation  and the source code. So convinced ? 😄

But if you use them with code which is not self-documenting, you won't be able to extract any information of the documentation generated, so it won't bring any value to you.

The second methodology discussed by the author is named *'Literate Programming'*. This paradigm states that the explanation  of the logic of the program should appear in the program. To achieve this, you should write in plain English the logic of your program/function, then, introduce the code inside the English explanation.
This is an extreme for of documentation which looks more like to a document annotated with source code than source code annotated with comments.
This paradigm has some advantages but also a lot of default, such as having to document important information as much as trivial information, for example.
Personally, I would recommend to use this paradigm if you want people to contribute on a project with you. It will require a lot of discipline to maintain such a paradigm and I think that this means justify the end. 😶

## Conclusion

Self-documenting code is really powerful.
It can make your code much more understandable for anyone arriving on your project, easier and cheaper to maintain. It will literally save you time and money to have self-documenting code when you will need to debug or extend it.

I don't know how I can sell you more the concept of Self-documentation 😆
But I hope now you will start to make your code more self-documenting, for you and the next developer to come after you.


Thank you all for reading this article,
And until my next article, have a splendid day 😉
