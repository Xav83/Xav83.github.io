# How to deal with errors ? - When your program face the hard reality of the world !

Hi dear reader, I'm Xavier Jouvenot and this is the sixth article about [Code Craft, by Pete Goodliffe](https://amzn.to/2ZrTaHQ).

If you want, you can look at the previous article about ['How to write comments'](https://10xlearner.com/2020/02/11/how-to-write-proper-comments-code-craft/), and you can find [book here](https://amzn.to/2ZrTaHQ).

## Why should I even care about errors ?

For a lot of programmers, as long as the code they write works in the few conditions they have tested on their machine, their job is done, and, any problems that shows up, come from the people that don't know how to use their code. They take pride in their code and refuse any critic over it. I may be a bit cynical, but sadly, I encountered this kind of behavior in some developers and you can also read about it in several books, like [_The Mythical Man-Month_](https://amzn.to/3ndcSBR).

I don't want you to not take pride in your work, there is obviously a huge benefits to be invested in your work and have some pride by the work you've accomplished so far.
But, and this is a huge "**but**", you should also be opened to critics, on the work that you have accomplished.
By doing so, you will be able to do an even better job in the future, and you will be able to take even more pride in your work.
A small take on your ego now, so that everyone, even you, will benefit about the critics and the improvements done on your past projects.

I have to admit that, I am guilty of that too for time to time.
Even if I have improved myself in that area, I can still be more welcoming about the critics, in order to give my best work out there.
I can only say that, I already see the benefit of this behavior in my life (professional and personal) compared to a few years ago, so I wish it will bring you the same benefits to you too

Enough of the personal aspects of it, let's dive into the technical aspect of this blogpost.

## Bugs VS Errors

As programmer, we must be able to classify the issues in the code we are working on, so that we have more informations about how o handle them.
Like the title of this part says, those issues can be sorted in two categories : the errors and the bugs.

The errors are the issues that come from the "outside world", the environment in which the program runs on (operating system, embedded environment, ...), from the environment you interact with (server, cloud, ...), or from the user.
On the other hand, the bugs group the errors previously defined, when they are not handle in any way, *and* the issue regarding the program not behaving as the programmers or the product designers want to because of an error in the code, or some misunderstanding between the programmers and the clients who have defined the behavior that the program must have. 

Our objective as programmers is to make our programs bug free, so we need to have system or procedures to recognized and handle errors when they arrived during the programs lifetime.

## Error Handling

There are several ways to handle errors in a program, and we are going to explore them together.

#### Return values

This technique is commonly used in languages like C, C++ or Java.

It consists in making your functions return an **error code**.
Doing so allows other people to know if everything happened as expected, or, if in case of problem, what kind of error happened.

```c++
enum class ErrorCode
{
  OK,
  BAD_INPUT,
  BAD_ACCESS,
  OTHER_ERROR,
};


[[nodiscard]] ErrorCode foo ();
```

With such code, any caller to the function `foo` will be able to know if everything happened normally or not.
But this technique makes the common way to return some data a bit more tedious.
Indeed, it requires either to use output parameters, or to craft a more complete output structure like:

```c++
enum class ErrorCode;

template<typename T>
struct ReturnVariable
{
  std::optional<T> output;
  ErrorCode error_code
};

[[nodiscard]] ReturnVariable<int> foo ();
```

This requires some discipline, so a lot of people in a team will not come up with some excuse to not do it ("too long to write", "too complex", "not optimized", ...).
So you may want to use such solution only in some specific part of you softwares, where it makes the most sense.

#### Error Status Variables

In some language, you can have access to a global variable for which the access is shared by all the functions of your program.
In C and c++, this variable is named [errno](https://en.cppreference.com/w/cpp/error/errno).

Compared to the returned value, this technique has the advantage to being used without polluting your function returns.
But even with this advantages, it has some big limitations.
Indeed, having a shared variable to determine if an error occurred or not doesn't give you the information about when the problem exactly occurred. If not checked regularly, this variable can described an error that have occurred a "long" time ago, or, in case of multithreading in your program, some error in one thread can be reported in another thread, or be omitted in the thread that supposed to be notified.

With such limitation and disadvantages, this solution is often not used in software, as the heaviness of the previous solution is far less problematic as the multiples issues from a shared error variable.

Even if you will not use this solution, it is always good to know that it exists, in case you encounter some code using it. :wink:

#### Exceptions

If you have play with several languages, you probably encountered some exception systems. They are very common in mainstream languages such as Java, C++ or Python, where this system is directly embedded in the language.

This system allows to separate the exceptional cases from the normal workflow of the program, by letting you interrupt it and handle the error in another place.

Such system has as the advantage to make your error system and the workflow of your program distinct so that one doesn't make the other more complicated to understand to new programmers, for example.
Even with this huge advantages, there are some major issues with this system such as the performance impact and the fact that, when handling an error, you can miss some information about the why this error happened.
Moreover, if there is always a risk that the code in your exception system, which is going to treat the error, to contain bugs.
Indeed, this code needs to be safe, really basic, strong and must not trigger another exception in case of failure. If it was able to trigger errors, then, this system would become very complicated very fast as you will have to catch errors from the normal workflow of the program and from the exception workflow.

Ideally, this system should only be used for unexpected events that should never occurs in the program lifetime.
For user or programmers errors, you should avoid using such a system, except if you want to make you daily work a lot more difficult :wink:

Sadly, in some languages, like C# or Java, this system is the main way to handle error, so that you will have little to no other choice. :tongue_out:

#### Signal / Interruption

Used to handle hardware events in embedded or very low-level systems (close from the metal of the machine), some people may think that this system can be used as an error handling system.

Indeed, this system works by stopping the program execution, executing the code defined for the interruption and resume the course of the program, so we can easily imagine using the code in the interruption to fix or to handle an error that occurred during the execution of the program, so that we could resume the program at the exact point where it was stopped.
Even if it can sound like a good idea by having the advantage of the exception (separation of the normal workflow and of the error workflow), it also have the same issues.

Indeed, the code in an interruption should be extremely short and safe. The less it will meet this requirement, the more chances of an error in the interruption code there is going to be.
Moreover, another interruption can happened when the code of an interruption is executed, which start to make handling errors more difficult and the code more convoluted.

Ideally, such system should only be used to receive information from the outside of the program, such as a system call, or a hardware interruption.
It has been design for it, and is very powerful in such condition, but for error handling, it will only make your code harder to read, and errors harder to fix.

#### Do nothing

Please! Please! Don't do that :pray:

Ignoring an error will, for sure, cause some problem later.
Even if you don't care if you program doesn't work because of a specific error, you should at least report it to the user or log it, so that, when it happens, the reason why the issue occurs will be clear to any people encountering it.

You can also look at the blogpost about [Defensive programming](https://10xlearner.com/2020/01/06/defensive-programming-code-craft/) talking about techniques to help you have a stronger code, which will be clear about errors when they occurs.

Whatever defensive mechanism you decide to use, if there are some errors you don't fix, because of some known unsupported system, for example, you should, at least report it to the user, or to yourself.

#### Mini conclusion

We have seen a lot of available solution to handle errors, but sadly, no silver bullet, and *spoiler alert*, there is no solver bullet in computer science.
Depending of the error you want to handle, you should prefer using the solution the most adapted, and prefer handling error right when you detect it instead of trying to handling later in the program execution. It will avoid you the pain of finding where the error happened.

And the more important of all, you should always manage errors clearly and explicitly by defining the responsibilities of each of your program structures/modules.
You work and the one of your colleagues will be far easier that way :wink:
Maybe using some routines or convention in you team or company could help you to achieve such goal.

## Reporting errors

Even if we have talk a lot about system allowing us to recover/fix some errors during the program execution, a lot of time, all we are going to want is to be able to state that an error occurred and stop the task in progress the more cleanly possible.

Several techniques can be used such as logging the problem, reporting it to the user though the user interface, or to your developers with assertions or one of the error handling system that we have seen previously.
Whatever the technique you use, you should try to give the more information you can to make the correction as easy as possible. The useful information can be:
- Where the error come from.
- What the code was trying to do before the error happened.
- Why this error happened.
- When, in the program lifetime, the error occurred.
- How writical is this error.
- If possible, how to fix this issue?

If you can give all those information when an error in your code happen, I guarantee that you will save a lot of time to the people (maybe yourself) that are going to experiment this error, and are going to try to find a way to fix it.

## Conclusion

Each of the ways described here to handle and/or report errors could be a blog post on its own, for any language.
My purpose with this article was to make you aware of the possible offered to you so that you will be, in the future, able to dig deeper into those.

I hope that it will reach its goal. :smile:

----------------

Thank you all for reading this article,
And until my next article, have a splendid day ���

## Interesting links

- [Code Craft, by Pete Goodliffe](https://amzn.to/2ZrTaHQ)
- [The Mythical Man-Month](https://amzn.to/3ndcSBR)
- [std::optional documentation](https://en.cppreference.com/w/cpp/utility/optional)
- [Defensive programming blogpost](https://10xlearner.com/2020/01/06/defensive-programming-code-craft/)
- [errono documentation](https://en.cppreference.com/w/cpp/error/errno)
- [10xlearner website](https://10xlearner.com)

