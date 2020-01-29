# Coding Standard - Code Craft

Hi dear reader, I'm Xavier Jouvenot and this is the second article about [Code Craft, by Pete Goodliffe](https://amzn.to/2ZrTaHQ).
If you want, you can look at the previous article about ['Defensive Programming'](https://10xlearner.com/2020/01/06/defensive-programming-code-craft/), and you can find [book here](https://amzn.to/2ZrTaHQ).

The chapter we will treat today, is named '*The Best Laid Plans - The Layout and Presentation of Source Code*'.

## What is a Coding Standard ?

If you go on [Wikipedia](https://en.wikipedia.org/wiki/Coding_conventions#Coding_standards), you can find a nice definition of a *coding standard* such as:
> Where coding conventions have been specifically designed to produce high-quality code, and have then been formally adopted, they then become *coding standards*. Specific styles, irrespective of whether they are commonly adopted, do not automatically produce good quality code.

The author of the [Code Craft](https://amzn.to/2ZrTaHQ) goes even further by saying that the purpose of a *coding standard* is too avoid common problems **and** unit the developer team, by the use of coding conventions.

Sounds cool, we all want to avoid problems and work in a united team, but then, you may wonder what is a *coding convention* ? And how to get some ? 🤔

Don't worry, this is what we are going to describe in this article 😉

## Coding convention

A coding convention is a rule or a set of rules which describe the use of the programming language that should be applied by all organisations following this convention. It can describe the features that should or not be used in some part of a program, the rules to name a variable, a function or a class, the layout of the code, the best practices, ... all the things that, if applied on your code base, can improve the quality of your software.

An example of coding convention can be where you place the braces when you code. Indeed, if you don't have any rule about braces, you can end up with code poorly formatted which will be hard to read and understand at the first glance. 🤔

Let's take this C/C++ example
```c++
if(condition == failure)
    log(error);
    exit(1);
```

Here you can easily imagine what the author was intended to do. If the condition was a failure, then he wanted to log the error and terminate the program. But in reality, the program will behave differently ! In case of failure it will log the error, but it will terminate the program in either way. Here is what the compiler understand:

```c++
if(condition == failure)
{
    log(error);
}
exit(1);
```

And here is what the author intended to do:

```c++
if(condition == failure)
{
    log(error);
    exit(1);
}
```

The braces made it clearer and could have avoid the original error of the author of this code. So a convention stating clearly what is the politic about braces in the source code is important. And this is only an example among a lot of issues that can be avoided.

## Which coding standard ?

In the previous part, we explained what coding standard and coding convention are, and now we are going to see which one to choose.

Indeed, there is no need for you to create a new coding standard for your team or your company from scratch, there are plenty of coding standard out there! For example, you have the [Indian Hill standard](https://www2.cs.arizona.edu/~mccann/cstyle.html), or the [GNU Misra standard](https://en.wikipedia.org/wiki/MISRA_C). The author also mentionned the some conventions for braces layout like the [K&R Brace Style](https://en.wikipedia.org/wiki/Indentation_style#K&R_style) and the [Allman, or Extended Brace Style](https://en.wikipedia.org/wiki/Indentation_style#Allman_style). All those standard can give you example of conventions that you can included or not in your projects.

Each standard has its own strength and weakness, and you should use a different one depending on what you use it for.
For example, you may not want to have the same layout if your code is displayed in an editor, or in a book or in a blog post.
You should be able to use one that suit the most your needs. 😉

#### Why so many coding standard ?

This is a great question that the author of the book mentioned.
He says (and I completely agree on this with him) that having only one standard will allow people to be more productive about thing that matters more, like producing software, fixing bugs, and all the things that brings value to a company, but having many standard has its own advantages like allowing standards to improve by try out ideas that may or may not become widely use conventions.

I really like the idea that all the debate between coding standard allow us to have better standards available to us. 🙂
Even some "Holy Wars" on which standard, convention, or language is the best 😆

## How to integrate a coding standard in your team ?

Depending on your project, there may already be a standard in place.

If it is the case, you should use it, and not try to force another one instead, that will only create tension in your team if you do so.
Indeed, people are extremely sensitive about the standard they use, so be careful if you want to integrate new conventions to your team coding standard. To do so, you should make sure that the convention you want to use can be apply **consistently** in all the files of your project, that this convention is **widely use** in other projects by the community (so that it is already battle tested), and finally, that this convention is **concise** and easy to use. With those quality, there would be more chances that the convention will be agreed by the member of your team, and used consistently.

If your project doesn't have a coding standard, on the other hand, you should propose to your team to create one.
But be cautious, this is a big task which have to be managed carefully to avoid unnecessary holy wars and pointless debate which will cause you to drop the coding standard before it is even implemented. 😝

But luckily for us, the author has been kind enough to give us a standard to create a standard in your team 😉
Here are seven rules of this standard:
- *Define the scope of the standard*. Too wide, it will contain garbage, to narrow, it will be too restrictive to be used.
- *Get people involved during the creation of the standard*. If people are involved early, they will want in implement it since it's partly their work.
- *Produce a document*. You standard should be accessible for people to refer too, and for new people to learn.
- *Includes team best practice*. It will encourage everyone if there are things that they already do.
- *Focus on what matters*. Avoid holy wars about tabs or spaces for example.
- *Start small and increment later*. Adoption will be far easier this way.
- *Compliment for adoption*. Use encouragement instead of punishing anyone that doesn't use the standard.

## Conclusion

Having a well defined standard is important.

It helps everyone in a team to unite against problem instead of focusing on where to put braces.
Moreover, if possible, you should definitely automate code layout when possible, to avoid conflict and code review about formatting.
Here are two links about formatting: [Formatting Cpp, C, Javascript and other stuff](https://10xlearner.com/2019/12/05/formatting-cpp-c-javascript-and-other-stuff/), [Formatting CMake](https://10xlearner.com/2019/11/15/formatting-cmake/).

I hope this article will help you like coding standard a little more, and maybe making you adopt one or several depending of your projects 😉

Thank you all for reading this article,
And until my next article, have an splendid day 😉
