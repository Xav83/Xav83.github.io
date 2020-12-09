# How easily and quickly can you specify arguments for your program in C++

Hello! I'm Xavier Jouvenot and in this small post, we are going to see how easily and quickly can you specify arguments for your program in C++.

_Self promotion_: You can find other articles on my [website](www.10xlearner.com) 😉

## Problematic

All programs don't have a graphical user interface, and even those whom have one can also be called using a shell/bash/batch command in the Terminal of you choice.
When you want to use the command line to run a program, most of the time, you can also pass some arguments to the program, and have it do some specific task it is programmed for.

Something like the command `ls -a` will modify the behavior of the command `ls` to also display the hidden repositories.

## Solution

We are going to look at two ways to do such thing in C++ : the "old" native solution and a modern one

### Native solution: argc and argv

This is the old and native solution to solve this problem in C++.
I say it is an old solution because it is inherited from C language which starts to be a very old language in the history of programming languages.

You have two variable handed to you directly as inputs in your main function : `argc` for the numbers of arguments of the command line, and `argv` which contains all the characters of the command line, already splitted into words.

```c++
int main(int argc, char* argv[])
{
    for(auto i=0; i< argc; ++i)
    {
        std::cout << argv[i] << std::endl; 
    }

    return EXIT_SUCCESS;
}
```

With this code, you can simply know what are the arguments passed to the program, and now, it is up to you, or us, as programmers to handle those variable properly to define what is accepted and what is not, and to tell the user, what it can and cannot do.
Doing so can become very difficult when you program grows larger and larger making it difficult for you and your collegues to maintain all the options that can be passed to your program this way, and to the user to know what it can and can not do.
Moreover, such interaction can easily lead to securities issues, so you better be sure that you handle this part of your program safely.

But, since it is native to the language, you can directly work on the arguments passed to your program 👍

With all that in mind, let's look at a modern solution available to us 😉

### Modern solution: [Argh!](https://github.com/adishavit/argh)

If you use some more modern technologies in your C++ development environment, then you should at least try this solution to process your command line.
[Argh!](https://github.com/adishavit/argh) is a header only library, easy to include, either by copy-paste, or via conan into your own build system or into a CMake project.
So no problem with that (you can also look at the details on the [documentation](https://github.com/adishavit/argh#finding-argh)).

Let's look at an example of this library in action:
```c++
#include <iostream>
#include "argh.h"

int main(int, char* argv[])
{
    argh::parser cmdl(argv);

    if (cmdl[{ "-v", "--verbose" }])
        std::cout << "Verbose, I am.\n";

    return EXIT_SUCCESS;
}
```

As you can see, this library actually uses the input `argv`, but, instead of you having to do all the parsing to figure out what the user has passed to you, the library actually does it for you. In this example, the program checks if there is either a `-v` or a `--verbose` flag, and, if this is the case, display a string to the user.

```c++
float scale_factor;
cmdl({"-s", "--scale"}, 1.0f) >> scale_factor; // Use 1.0f as default value
```

Moreover, you can easily allows the user to specify a value for a flag with two lines of code.
With this example, and the command line `my_program -s=69`, you will be able to set the `scale_factor` variable to a nice value :)

Of course, there are a lot of other things that you can do with this library, and I encourage you to take a long look at its [documentation](https://github.com/adishavit/argh).

I hope it will simplify your job a lot by not having to parse your program input "by hand". 😉

-----------

Thank you all for reading this article,
And until my next article, have a splendid day 😉

## Interesting links

- [Argh website](https://github.com/adishavit/argh)
- [CMake website](https://cmake.org/)
- [Conan website](https://conan.io/)
- [Conan Argh package](https://conan.io/center/argh)

