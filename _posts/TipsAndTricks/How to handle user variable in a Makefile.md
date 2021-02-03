# How to handle user variable in a Makefile

Hello ! I'm Xavier Jouvenot and in this small post, we are going to see how to handle user variable in a Makefile.

_Self promotion_: You can find other articles on my [website](www.10xlearner.com) 😉

## Problematic

When creating some make commands in your Makefile, you may want to have some parameters accessible to the user, so that he could either pass some inputs to your command, or specify some elements for which you will have set some default value.

There are several ways to do it, but we are going to see the simplest and most powerful solution to me.
Hopefully, you will find it convenient too 😉

## Solution

Let's dive right in the Makefile and see what defining a user variable looks like:
```Makefile
VARIABLE ?= DEFAULT_VALUE

command:
    echo "${VARIABLE}"
```

Pretty simple, isn't it ?! Yet, if you don't know it exists, you can't guess it 😆
Everything is done by the operator `?=` which is going to set our `VARIABLE` only if it has not been defined before.

Now, all we need is to pass a value to the variable like that:

```shell
make foo # uses the default value specified in the Makefile for VARIABLE
make foo VARIABLE= # set VARIABLE to an empty value
make foo VARIABLE=SPECIFIC_VALUE # set VARIABLE to a SPECIFIC_VALUE
```

And voilà now, you know that the operator `?=` exists and how to use it 😉

----------

Thank you all for reading this article,
And until my next article, have a splendid day 😉

## Interesting links

- [Makefile definition on wikipedia](https://en.wikipedia.org/wiki/Makefile)
- [GNU manual - operator ?=](https://www.gnu.org/software/make/manual/make.html#index-_003f_003d)
