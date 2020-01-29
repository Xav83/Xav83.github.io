# The Power Of Naming - Code Craft

Hi dear reader, I'm Xavier Jouvenot and this is the third article about [Code Craft, by Pete Goodliffe](https://amzn.to/2ZrTaHQ).
If you want, you can look at the previous article about ['Coding Standard'](https://10xlearner.com/2020/01/15/coding-standard-code-craft/), and you can find [book here](https://amzn.to/2ZrTaHQ).

The chapter we will treat today, is named '*What's In A Name - Giving Meaningful Things Meaningful Names*'.

## Why naming matters ?

In programming, we consistently name things (variable, functions, types, modules, source files, ...) and even if those names doesn't really matter to the compiler, as long as the syntax is good, it is important for you and other developers. 🤓

Indeed, the more information we can get as humans, the more we are able to understand the logic, the patterns and the behavior behind a source code or a software architecture. And no need of an elaborate example to see this ! If you see the instructions `int a = 42;` and `int apple_count = 42;`, you can instantaneously get information about what the variable represent in the second one, but not in the first one.

So using good naming definitely bring value to yourself and your collaborators when working on a piece of code. On the other hand, bad naming will make code more difficult to understand and reason about, the intention of the author of the code will be more difficult to understand, and you won't be able to make any assumption on any part of the code.
Indeed, if you meet a function `bool isInferior (int expectedSmaller, int expectedMax);` that returns true when the two parameters are equals, then it can trigger unexpected behavior in your code while it work perfectly for the author of this function, but naming it `isInferiorOrEqual` would have make it clearer for everybody, and less error prone. 😉

### The value of a good naming

As the author of the book said : *"to know the name is to know the object"*. This quote is full of truth ! 🙂👍

Names can gives you information about the **expected behavior**, allows you to recognized the same piece of information through the code and allows you to manipulate easily similar objects without difficulty by identifying them clearly. To accomplish that, a name must have the following qualities :
- to be descriptive
- to be technically correct and idiomatic (for the language **AND** for the coding standard you are using)
- to be appropriate (avoid abbreviation and jokey names)

To create your own names or to spot them, the author gives you three questions to ask yourself:
- What are you naming ?
- How will it be used ?
- Why does it exists ?

If the name you have can answer those three question easily, then you have a good one ! 😉

## Advises to name things

To help us crafting better names, the author gives us some more specific advises depending on the elements we want to name.
First of all, I should mention that it is necessary to use good coding conventions which specifies some elements of the naming process, such as the [Camel Case convention](https://en.wikipedia.org/wiki/Camel_case) or the [Proper Case convention](https://www.computerhope.com/jargon/p/proper-case.htm). Now, let's look at some of the advises:

- for the **variables**, we should prefer nouns or "noun-ized" verbs, avoid acronyms (or use it only for small scopes) and avoid restating the type of the variable in its name.
- for the **functions**, we should prefer verbs or phrases starting with one, describe the logical operation, not the implementation, and avoid generic verb such as "be", "do" or "perform".
- for the **types**, we should either use a noun for data structure or a verb for function object, mentioned the design pattern use, if there is one, and avoid meaningless term such as "data", "object" or "class".
- for the **source files**, we should be careful with the case, and try to have unique names, even for files in different folders.

Finally, the author mentioned that we should use namespace, package and/or modules (depending on what is available in your language) to avoid name conflict between the different parts of your project and the libraries you use.

## Rules to have a good naming

To finish, the author gives us three rules to follow to get the best of good naming:
- be consistent
- exploit context
- gives the more information possible

Don't worry, I will add some details 😉

The first rule allows us to facilitate work by making the code easier to work with, extend, maintain and understand.
The second one makes our names shorter by using the context given by the surrounded elements, such as the scope you are in, or the type you are using. And the last, but not least, remind us to be as descriptive as we can, and push it even further by giving us the tip of using prefixes or suffixes to create links between element that are related.

Pretty nice rules, aren't they ? 😉

## Conclusion

As a wise man in a comic book once said, "with a great power comes great responsibility". 🕷

Naming really gives us a power !
When use wisely, it can bring peace and knowledge to a whole team, but if done poorly, it can bring suspicion, mistrust about your and other people code.
I hope you are know convinced of the importance of using good naming in your code, and at least, after reading this article, you have been warned 😉


Thank you all for reading this article,
And until my next article, have an splendid day 😉
