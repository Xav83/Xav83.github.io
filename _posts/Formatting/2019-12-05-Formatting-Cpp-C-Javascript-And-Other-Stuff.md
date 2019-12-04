# Formatting Cpp, C, Javascript And Other Stuff...

Hello dear reader, if you ever came to this article wondering if you should format your code, you can stop reading right now, and go check [my other post](2019-11-07-Formatting-And-Automation.md) on that topic.

But if you are convinced that code should be formatted, and automatically formatted, first of all welcome 🙂 this post is made for you (and for the future me if I need it 😄)

In this post, I am going to talk specifically about formatting C++ code, but this can be also used to format  C, Java, JavaScript, Objective-C, Protobuf and C# languages.
Why C++, you may ask ? For no particular reason other than this is a language I use almost daily 🙂

Enough for the introduction, let's go in the center of the subject !

## How to format with C++ code ?

If you google ["How to format C++ code"](https://lmgtfy.com/?q=How+to+format+C%2B%2B+code), you will find a lot of articles and posts about tools you can use to format C++ code, which can make it difficult to choose what to do.

But there is one tool that comes back a lot of time, a tool named `clang-format`, and we are going to see how to use it to format our code.

Why this one, you may wonder ?! 🤔

Well, if you build c++ code, you have probably encountered the [clang compiler](https://clang.llvm.org/docs/UsersManual.html#introduction), which is an open-source compiler for the C family of programming languages.
I won't go in detail on the `clang` project, but as part of this project, there is the tool `clang-format` which is used to format C++ code.

That means that `clang-format` is actively maintained, which is really great !
Moreover, `clang-format` allows you to format more that C++ code ! It currently allows you to format C, C++, Java, JavaScript, Objective-C, Protobuf and C# languages. So even if you don't use C++, there is still a chance that this tool can be useful for you. In the rest of this article, each time I will talk about using `clang-format` for formatting C++ code, you can think that it can do the same for those other languages.

It also allows you to customize the formatting to have the one you like for your projects, and you can integrate it inside a [lot of others tools](https://clang.llvm.org/docs/ClangFormat.html#vim-integration)!

For all those reasons, let's see how to actually use it 😃

### How does it work ?

Good question ! For those who have ever use [cmake-format (a tool to format Cmake code)](2019-11-14-Formatting-Cmake.md), it works almost the same way (even if `clang-format` has been created before 😆).

Once installed, you can use it in your terminal like that to format a file:

```bash
clang-format -i myfile.cpp
```

Pretty straight forward, isn't it ?!
The option `-i` allows `clang-format` to write the formatted version of the Cmake file in place, which means that, after you run this command, your file is now formatted 😃

There is a lot of option available with this tool, and I won't go too much in details, but I invite you to look at them on his [GitHub](https://clang.llvm.org/docs/ClangFormat.html#standalone-tool) 😉

Moreover, you can create your how configuration file, that you can name `.clang-format`, if you want, to specify how you want some elements to render in your C++ files in a folder.

So you can format your C++ code and customize how it will be formatted ! How awesome is that ?! 😃

### You convinced me ! How do I get it on my machine, now ?

The better way to install clang-format, is to go through a package manager.
Of course, you can build it from the llvm sources, but this will take a long time and probably needs its own article. So the package manager is the way to go.

And the good news is that, whatever the Operating System you are using, you can install it through a packkage manager:
- [Linux](https://apt.llvm.org/) - `apt-get install clang-format`
- [Osx](https://formulae.brew.sh/formula/clang-format) - `brew install clang-format`
- [Windows](https://chocolatey.org/packages/llvm) - `choco install llvm`

Or you can install it with `pip`(https://pypi.org/project/clang-format/) on whatever system you want : `pip install clang-format`.

## How to automatically format a repository ?

Now that we know how to install `clang-format`, and use it to format a file, let's see how to automatically format all the C++ files of in a repository (for other language, you can use a similar method and only adapt a few characters).

If you know exactly what are the C++ files your project have and are sure that those will never change (no new files), you can only use the command line showing you how to use `clang-format` to format them all.
But, in most project, some files will be added, moved or removed, so we need a way to format them all, without knowing their location or their name in advance.

To do so, a simple command line can do the trick by combining the command `find` and `clang-format`, like that:

```bash
find . \( -name '*.hpp' -o -name '*.cpp' \) -exec clang-format -i {} \;
```

This command will look through all the folder and sub-folder from your current position, and look for the files with the extension `.hpp` or `.cpp` files. Then it will place all those files in place of the `{}` and run the command `clang-format -i` with all the files. If you are using other extension for the files, you can add or remove them util all the files you want to format are given to the command line !

And voilà, you have it ! One line, you can format them all 😉

## And there is more !

The one line command to format all the C++ files is good, but, I must say that this is not a line easy to remember.
This issue can prevent people to use it because they don't want to go through the documentation to find it again to paste it in their terminal.
And we can't be mad at them for that, if they can, people will always resist change, so if you have such line to learn without being convinced of the benefits of formatting, you won't learn it.

So what to do ? We have only one line ? Can we make it simpler ?

And yes, we can ! 😉👍

To do so, you can create a [Makefile](https://gl.developpez.com/tutoriel/outil/makefile/), on the top level of your project repository, so that you will have an alias for this complicated line. This `Makefile` will be as simple as that:

```Makefile
format:
	set -f
	find . \( -name '*.hpp' -o -name '*.cpp' \) -exec clang-format -i {} \;
	set +f
```

And now, all people will have to do is to run `make format`, and all the formatting will occur without them having to learn a complicated line.

This is actually more powerful than that ! If for some reason, you have to change the tool you use for formatting C++ files, all you have to do, is to modify the `Makefile`, and your coworkers won't even be bother by it, since the command `make format` will still be the thing they will have to do !
Moreover, if you want, you can also add other formatting tools for other language in the `Makefile` so that the command `make format` formats actually all your source files, whatever their languages !!! 😍

I also have to mention that, to help to integrate it to your workflow, `clang-format` can also be include in a lot of other tools, such as [Vim](https://clang.llvm.org/docs/ClangFormat.html#vim-integration), [Emacs](https://clang.llvm.org/docs/ClangFormat.html#emacs-integration), [BBEdit](https://clang.llvm.org/docs/ClangFormat.html#bbedit-integration), [CLion](https://clang.llvm.org/docs/ClangFormat.html#clion-integration), [Visual Studio](https://clang.llvm.org/docs/ClangFormat.html#visual-studio-integration), and probably a lot more if you search for them. 😉

## Let's force formatting by default !

Now, we have an easy-to-remember command than allows us to format our C++ code.
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

You can find [here](https://gist.github.com/alexeagle/c8ed91b14a407342d9a8e112b5ac7dab), an exemple of pre-commit hook that may help you to have your own 😉

### CI check

Another way to force the formatting of the files on your repository, is to create a check on a CI/CD system such as [Travis](https://travis-ci.org/).

If you are not familiar with the concept of [Continuous Integration](https://en.wikipedia.org/wiki/Continuous_integration), I encourage you to look at it, it will truly benefit you, and your team. For those who know what a CI/CD system such as Travis is, let's continue.

But how can we test that the files of our repository are well formatted ?
Pretty easily actually, all we have to do is to implement this three steps:
- Getting `clang-format` on the system, if it is not already installed
- Running the format command `make format`
- Checking that the formatting didn't change any files

If we can implement all of them, we win 🙂

Actually, we already have 2 of them !
We know that we can install `clang-format` using `pip` ! This is a first win 😉
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
    - name: Running cpp formatting
      run: make format
    - name: Checking formatting
      run: git diff --exit-code
```

No need to install clang-format since it is already install by default on the system provided by GitHub Action \o/

## Conclusion

As a conclusion, I will say that writing this article was pretty fun.
I sincerely hope that you had as much fun reading it that I had writing it 🙂

And that it will help you to have a better workflow (at least for the formatting part) in your code.
I will write some other article to help you and myself format files of other languages.
They will be out in the next weeks.

And until then, have fun learning, growing, and have a wonderful time. 😄
