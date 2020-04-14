# Encapsulation as a defensive programming technique

Hi dear reader, I'm Xavier Jouvenot and, in this article, we are going to talk about Encapsulation, more specifically in C++, even if the concept can be extended to other languages. This post was inspired by a rule from the first chapter of [Code Craft, by Pete Goodliffe](https://amzn.to/2ZrTaHQ), on [Defensive programming](https://10xlearner.com/2020/01/06/defensive-programming-code-craft/)

## What is it ?

The term `encapsulation` means *"to enclose in a capsule"*. In computer programming, `encapsulation` means that you *enclose*, or hide an element inside another one to prevent unauthorized parties to access to it.
This mean that you can make your data be used only by you, and force the others to use the interface that you will have defined.

Concretely, it allows you to create a module, a black box, where all the data and the internal behavior of the module is hidden to people who want to use this module, and those people can only use your interface to use this module.

The encapsulation is used extensively in Oriented Object programming, where each object can have data and encapsulate them easily.
It can also be used in other programming paradigm, if you are discipline enough to have modularized your code, and use the interface created to use those module.
Actually, the same discipline is necessary in Oriented Object programming, since some developers won't hesitate to bypass your interface whenever they can.

## Pros and cons

There are some advantages and disadvantages to use encapsulation in a program.
I'm convinced that the advantages outweigh completely the disadvantages, and I think you will be convinced about that too when you will have read about them.

### Cons

Let's start with the disadvantages.

The first one is the impact that encapsulating elements have on performance.
Indeed, when you add a method to access an element, you add one level of indirection before getting this element.
Moreover depending on how you encapsulate the different elements of your program, aka your program architecture, you may need to go through all your architecture to get the data you need, which will impact your performance even more.

But this problem can be avoided by designing your software architecture more carefully, maybe even using some [Design Patterns](##Generic%20solution%20:%20Design%20Patterns).
Moreover, don't encapsulate things just for the sake of encapsulating them. This may be only my personal opinions, but, some "best practices" tell you to always create getters and setters for each of the attribute of a class, which can be great if you have some constraints, some check to do before actually setting the attribute, whereas is totally useless, and only slow down your program, if you only set and get the attribute without adding any value to it. In that case, you are probably better letting this attribute "public", available to everyone.

The other problem with encapsulation that I want to mention is that, it can make your program harder to read when it is poorly done, or done for the wrong reason.
Indeed, to make your encapsulation useful, you need to use [good names](https://10xlearner.com/2020/01/21/the-power-of-naming-code-craft/) to explain clearly your intention when your encapsulate a data. By that I mean that the methods created to interact with this data should always state what the intention behind this method is, so please, avoid name such as `modify<nameOfYourAttribute>` or `set<nameOfYourAttribute>` if you haven't stated clearly, with comment or another name, that this method will check for the constraints on the attribute or if they have some side-effects. And as I said earlier, don't encapsulate for the sake of it, but do it with a purpose in mind.

### Pros

Now that we have seen the disadvantages, I hope that you are ready for the advantages of using encapsulation in your program.

First of all, encapsulating allows you to modularize the different parts/components of your program, which is really important.
Indeed, being able to reuse code is a huge benefit. It allows you to reduce your code size, facilitate refactoring, maintenance, and even improve your code performance.

Moreover, since you reduce your code size by creating modularizing your program, your code become more easily understandable for any new comers on your code base.
And it give you the capacity to implement the [SOLID Principles](https://en.wikipedia.org/wiki/SOLID) to have an even more readable and stable code, with clear and defined intentions for each of your modules.

Finally, encapsulating your data mean that you are reducing the scope where those data will be used.
This will definitely help you improve the performance of your code and help you to protect your data, since they will be available only for the smallest moment to your program, giving less opportunity to other developer to using your code badly 😆

### Small conclusion

It may be only my opinion, but the advantages outweigh largely the disadvantages.
Moreover, each disadvantages can be avoided if you are careful about it.

## Generic solution : Design Patterns

Encapsulating you data in different manners can help you solve different problems.
But since it's 2020 and people have come with some elegant and generic manners to resolve some kind of problems by creating patterns, named Design Pattern.

Those Design patterns can be used as template to solve a lot of problems that we, as developers, can encounter.
They are often strongly linked to Oriented Object programming, like C++ or Java, since they often used techniques such as inheritance, and composition to create clear and easy to use interfaces for the developers to use while solving most of the problem for you.

The Design Pattern [Facade](https://sourcemaking.com/design_patterns/facade) is a perfect example of Design Pattern made to solve some encapsulation problem.

I can only encourage you to look through [all of them]([Design Pattern Sourcemaking](https://sourcemaking.com/design_patterns)) or on [Refactoring Guru](https://refactoring.guru/design-patterns/what-is-pattern), learn what they are useful for, so that you can improve as a developer by knowing when and where to use them. 💪

## Conclusion

As a conclusion, as programmer, we often use encapsulation techniques without knowing it.
But if we want to improve on our craft and be more conscious about what we are doing, it is important to learn to recognize and implement those techniques.

I hope I have convinced you about the importance of encapsulating your data, and to learn more about different techniques of encapsulation, such as the Design Patterns, in order to create strong and reusable data structures in your code. 🙂

Thank you all for reading this article,
And until my next article, have a splendid day 😉

## Interesting links

- [Encapsulation definition](https://en.wikipedia.org/wiki/Encapsulation_(computer_programming))
- [Design Pattern sourcemaking](https://sourcemaking.com/design_patterns)
- [Refactoring Guru](https://refactoring.guru/design-patterns/what-is-pattern)
- [SOLID Principles](https://en.wikipedia.org/wiki/SOLID)
