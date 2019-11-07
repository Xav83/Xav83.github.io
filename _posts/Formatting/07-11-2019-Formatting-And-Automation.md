# Formatting and Automation

Who during code review didn't have a coworker telling you "Please use braces one the same line as the if condition, if not, it's unreadable", or the opposite, "Here we place the bracket after the if, the code is more clearer that way", or the classic one, "Do not use tabs, use spaces for indentation"... I don't even talk about the arguments around those "important" rules, as if your compiler, or your interpreter gave a f*** about it when creating your program.

Indeed there are some programming language where a space instead of a tab can change the behavior of the program (no, I don't want to talk about the [Whitespace programming language](https://en.wikipedia.org/wiki/Whitespace_(programming_language)) 😝), but most of them do not care about it. And when the interpreteur in popular programming language (yes [Python](https://en.wikipedia.org/wiki/Python_(programming_language)), I think about you) cares about it, then people stop arguing, and code with those restrictions.

So why do people argue about that when offered the chance ? Is there a good answer to the formatting questions ? And is there a solution to end it once and for all ? That's what we are going to discuss in this article 😉

## Why such fight ?

I could just argue that all humans seems to be like is to fight over anything, but this is an entirely different topic.
Let's focus on other thing than human nature, and start with why formatting can be seen as such a big deal for some people.

### Source code size

In a galaxy not so far away and not so long ago, computers didn't have a lot of memory, nor processing power. 👵
You can take as example the size of the game [Super Mario Bros. compared to a screenshot taken on a modern computer](https://www.truthorfiction.com/was-the-original-super-mario-bros-only-40-kb-while-modern-screenshots-of-one-frame-are-283-kb/) to see the gap between them, or the [memory needed to send people on the moon](https://www.metroweekly.com/2014/07/to-the-moon-and-back-on-4kb-of-memory/). Those are popular examples, but you can easily imagine that, if you only have 4 Kb of memory for your program to run, at the time, your source code would not be stored on a 1 terabyte hard drive. You can look [IBM System/360](https://en.wikipedia.org/wiki/IBM_System/360#Table_of_System/360_models) to get convince about the small space available on the old machines.

So, at that time, using tabulation or spaces could have mattered since a tabulation instead of 2 or 4 spaces, on an entire program will reduce the size of you source code; keeping the bracket on the same line as the if condition avoid using memory to store a new line. Those elements add up in a program and at a time when the memory was so expensive and so limited, keeping your source code short in size was important.

**BUT** this constraint doesn't exist anymore. For most of projects, you won't have more the 1 TB of source code, so you should be OK. 🙂

### Readability

Who has ever read some code, thought "Who has written this ! This is not understandable !" and when running a `git blame` on it, you were like "Oh... that was me". Sound familiar, isn't it ?
So, now, you have to spend 5, 10, 15 minutes or more to understand this code before looking and the rest of the code which is probably written as badly.

What if you are looking for the origin of a bug ? If you are lucky, you have the origin of the bug under your eyes, and you have to decrypt it. If not, you are going to decrypt some code which is not even related with the bug. Whatever your chance, this poorly written code made you loose time and money (for the time you spend on it).

But what poorly written code has to do with formatting you may wonder.
Indeed, you can write ugly but well formatted code and elegant code not formatted correctly.
Formatting your code won't make it magically understandable, but it will definitely help you understand it quicker since you won't have to deal with it.

#### But what is a well formatted code ? What does it look like ?

Currently, there is no ultimate formatting for source code.
People still argues about it to make formatted code clearer, prettier for the eye.

But this is not something that should stop you to format your code.
In fact, the more important is not what kind of formatting you use, but rather the fact that you are using one and that you use it consistently all over your project.

Indeed, if in a single file, there are 5 styles of formatting, when reading it, you will have to adapt yourself each time you go from on to another. But if you have only 1 style of formatting in your whole project, you will only have to adapt once if you are not already used to it. And on that poorly written code, the formatting won't get in the way of your understanding anymore.

## How to implement a formatting style in a project ?

So OK, we need to format our code to gain some time to understand more easily the code we read.
And we need to be consistent, which means, we have only one kind of formatting (one standard) by project.

But how to enforce it ?
How to be sure that the code written by any collaborator on the project is well formatted ?

Of course, you can review each Pull Request, to see with your own eyes if the code is formatted as it must be in this project.
But it's time consuming, and you may even have some collaborators trying to argue about it, and it will be even more time consuming. ⏲

So you win time on the readability, but you loose time when integrating and reviewing new code.
Not a good trade, isn't it ? So what, should we quit formatting ?

No, the solution is automation.

### Automatic formatting

Why having a way to automatically format your code is the way to go ?

First of all, the automation of the code formatting is going to remove the need of you reviewing the code only to find formatting errors. So you gain some time back. ⏲💲

Moreover, the collaborators will be less likely to argue and try to impose another formatting if all they have to do is to run a command to format their code to the format of the project.
And like that, you gain more time back. ⏲💲

And as bonus, even if you or another developer make a formatting mistake, the automating formatting will fix it.
And we can go further ! You can write code without taking care of the formatting, only run the automated formatting command, and voilà, your code is formatted ! So again, some more time in your hands. ⏲⏲⏲💲💲💲

Finally, you can style argue about formatting if you want.
To do so, you only have to go debate with the people building the tools allowing you to format automatically your code.
By doing so, you will help the community to improve their formatting and everybody will benefit about that without slowing down the other projects your work on.

## Conclusion

If it is not clear enough, you should definitely format your code.
I say even more ! You should definitely format **automatically** your code.
It will help you gain some time and avoid comments about formatting, and only focus on the content, the purpose on the source code.

At this point, you may be convince about the need of automatically formatting, but wondering how to achieve it concretely.
And this is when I say to you that in the next weeks, I will publish small articles about the tools you can use to automatically format your code so that you won't have to worry about how to do it, and have examples to guide you 😉

Until then, have fun learning and growing.
