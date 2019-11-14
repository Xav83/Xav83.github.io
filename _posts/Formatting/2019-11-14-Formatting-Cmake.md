# Formatting Cmake

Hello dear reader, if you ever came to this article wondering if you should format your code, you can stop reading right now, and go check [my other post](2019-11-07-Formatting-And-Automation.md) on that topic.

But if you are convinced that code should be formatted, and automatically formatted, first of all welcome 🙂 this post is made for you (and for the future me if I need it 😄)

In this post, I am going to talk specifically about formatting Cmake code.
Why Cmake, you may ask ? For no particular reason other than this is a language I use almost daily 🙂

Enough for the introduction, let's go in the center of the subject !

## How to format with Cmake code ?

If you google ["How to format Cmake code"](https://lmgtfy.com/?q=how+to+format+cmake+code&s=g), you won't find a lot of solution.

Indeed, there isn't a lot of program that allow you to do it. But luckily  for us, someone have created a program do to exactly that, and it works pretty well and is still maintained ! 😃

This program is named `cmake-format` and its source code can be found on [GitHub](https://github.com/cheshirekow/cmake_format) if you want to look at it.

### How does it work ?

Good question ! For those who have ever use `clang-format` (a tool to format C, C++ and other kind of code), it works almost the same way.

Once installed, you can use it in your terminal like that to format a file:

```bash
cmake-format -i myfile.cmake
```

Pretty straight forward, isn't it ?!
The option `-i` allows `cmake-format` to write the formatted version of the Cmake file in place, which means that, after you run this command, your file is now formatted 😃

There is a lot of option available with this tool, and I won't go too much in details, but I invite you to look at them on his [GitHub](https://github.com/cheshirekow/cmake_format#usage) 😉

Moreover, you can create your how configuration file, that you can name `.cmake-format.json` for example, if you want, to specify how you want some elements to render in your Cmake files in a folder.

So you can format your Cmake code and customize how it will be formatted ! How awesome is that ?! 😃

### You convinced me ! How do I get it on my machine, now ?

Tell me if this remind you sometimes:
You found a great tool, it does everything you want, but after two hours, you still have been able to install it or to make it work on your machine...

Sounds familiar ? For me, it does 😝

But it is not the case of this tool ! \o/
All you have to do, is to install it with [pip](https://pypi.org/project/pip/), the package installer for Python. If you are not familiar with [pip](https://pypi.org/project/pip/), I really encourage you to check it ! It will become really helpful if you need to create script to automate some task, for example.

## How to automatically format a repository ?

Now that we know how to install `cmake-format`, and use it to format a file, let's see how to automatically format all the Cmake files of in a repository.

If you know exactly what are the Cmake files your project have and are sure that those will never change (no new files), you can only use the command line showing you how to use `cmake-format` to format them all.
But, in most project, some files will be added, moved or removed, so we need a way to format them all, without knowing their location or their name in advance.

To do so, a simple command line can do the trick by combining the command `find` and `cmake-format`, like that:

```bash
find . -name '*.cmake' -or -name 'CMakeLists.txt' -exec cmake-format -i {}
```

This command will look through all the folder and sub-folder from your current position, and look for the files with the extension ".cmake" or for the `CMakeLists.txt` files. Then it will place all those files in place of the `{}` and run the command `cmake-format -i` with all the files.

And voilà, you have it ! One line, you can format them all 😉

## And there is more !

The one line command to format all the Cmake files is good, but, I must say that this is not a line easy to remember.
This issue can prevent people to use it because they don't want to go through the documentation to find it again to paste it in their terminal.
And we can't be mad at them for that, if they can, people will always resist change, so if you have such line to learn without being convinced of the benefits of formatting, you won't learn it.

So what to do ? We have only one line ? Can we make it simpler ?

And yes, we can ! 😉👍

To do so, you can create a [Makefile](https://gl.developpez.com/tutoriel/outil/makefile/), on the top level of your project repository, so that you will have an alias for this complicated line. This `Makefile` will be as simple as that:

```Makefile
format:
	set -f
	find . -name '*.cmake' -or -name 'CMakeLists.txt' -exec cmake-format -i {} \;
	set +f
```

And now, all people will have to do is to run `make format`, and all the formatting will occur without them having to learn a complicated line.

This is actually more powerful than that ! If for some reason, you have to change the tool you use for formatting Cmake files, all you have to do, is to modify the `Makefile`, and your coworkers won't even be bother by it, since the command `make format` will still be the thing they will have to do !
Moreover, if you want, you can also add other formatting tools for other language in the `Makefile` so that the command `make format` formats actually all your source files, whatever their languages !!! 😍

I also have to mention that, to help to integrate it to your workflow, there is a [Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=cheshirekow.cmake-format), and a [Sublime Text extension](https://packagecontrol.io/packages/CMakeFormat) for `cmake-format`. This can help you, but if one of your collaborator doesn't use those text editor, they won't be able to enjoy those extension.

## Let's force formatting by default !

Now, we have an easy-to-remember command than allows us to format our Cmake code.
Everybody is happy, and use it.
Everybody, well actually no, there is this one time you forgot, and the code has been integrated to the project, and now, everybody has formatting conflicts.
Or that time when the new intern didn't know he (or she) that this command existed.
Or those coworkers that want to use it, but still forgets, since they don't have the habit yet

For all this reason, and maybe more, you should for the formatting by default.
But what do I mean by that ? 🤔
It's simple, you should make it impossible, or at least, very difficult for anyone to not format his code, and have this not-formatted code in be integrated in production, or even reach code reviews !

How do we do that ?
Well, there is actually two things you can do.

### Pre-commit hook

If you are using `git` (and I hope you do version your code), you can create what is called a [pre-commit hook](https://githooks.com/). This is a script that is going to be executed before each of your commits, which is perfect for formatting !

Indeed, if, before any of your commits, you can format automatically your code, then, you won't ever forget it, or even have to thing about formatting again! 😃
This is how you gain back some time to do some more important stuff: automation !!

The author of the `cmake-format` repository has even been kind enough to give you a pre-commit ready to use! So nice of him ! ❤️

You can find it [here](https://github.com/cheshirekow/cmake-format-precommit)

### CI check

Another way to force the formatting of the files on your repository, is to create a check on a CI/CD system such as [Travis](https://travis-ci.org/).

If you are not familiar with the concept of [Continuous Integration](https://en.wikipedia.org/wiki/Continuous_integration), I encourage you to look at it, it will truly benefit you, and your team. For those who know what a CI/CD system such as Travis is, let's continue.

But how can we test that the files of our repository are well formatted ?
Pretty easily actually, all we have to do is to implement this three steps:
- Getting `cmake-format` on the system
- Running the format command `make format`
- Checking that the formatting didn't change any files

If we can implement all of them, we win 🙂

Actually, we already have 2 of them !
We know that we can install `cmake-format` using `pip` ! This is a first win 😉
And running the command `make format` is as simple as executing any other command on a CI/CD system.

So all we have left, is to check if one file as change after the formatting, which will mean that the person did not have formatted his code before the commit or the push on the CI.
And there is a command line for that too 😉

```bash
git diff --exit-code
```

This command will check the difference for the files in your repository, and will return 0 (= success) if no change is detected, and something else (=failure) if a file has change.

And voilà, we have our 3 steps 😃

#### [GitHub Action](https://github.com/features/actions)

To help a bit more, you will fin below this line, the content of a [GitHub Action](https://github.com/features/actions) which does exactly that ! Since GitHub Actions is now available, I think that it might be easier for anyone to integrate it that way. 😉

```yml
name: C/C++ CI

on: [push]

jobs:
  build:

    runs-on: windows-2016

    steps:
    - uses: actions/checkout@v1
    - name: Installing dependency
      run: |
        python -m pip install --upgrade pip
        pip install cmake_format
    - name: Running cmake formatting
      run: make format
    - name: Checking formatting
      run: git diff --exit-code
```

## Conclusion

As a conclusion, I will say that writing this article was pretty fun.
I sincerely hope that you had as much fun reading it that I had writing it 🙂

And that it will help you to have a better workflow (at least for the formatting part) in your code.
I will write some other article to help you and myself format files of other languages.
They will be out in the next weeks.

And until then, have fun learning, growing, and have a wonderful time. 😄
