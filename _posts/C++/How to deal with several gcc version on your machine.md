# How to deal with several gcc version on your machine

I'm Xavier Jouvenot and in this small post, we are going to see how to deal with several gcc version on your machine.

_Self promotion_: You can find other articles on my [website](www.10xlearner.com) 😉

## Problematic

When working on several projects, or even on one project, you may need to use different versions of a same program.
Indeed, for some programs or libraries, you may want to support several version of a program and be able to test it on your machine.

This problem can be even more of a preoccupation if you maintain a library as you may want for your users to be able to use it with several versions of compilers like gcc.
And this is exactly what we are going to focus on in the next part.

## Solution

Let's say that you have 2 versions of gcc and g++ installed on you machine, the version 10.1 and 9.3.
By using the command `update-alternatives` with different options, you are going to be able to switch easily between gcc and g++ versions.

```bash
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-10.1 1
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-9.3 2

sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-10.1 1
sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-9.3 2
```

In this sample of instructions, we use the `--install` command to reference the gcc and g++ version available on our machine.
We precise the location of the link to gcc and g++, the name of the program ("gcc" and "g++"), the location of the executable of each version of the programs and the priority of use, which mean that, by default, in our example, the version 10.1 will be used on our machine when calling gcc or g++.

If you ever want to select another version than the one with the highest priority, you can use the command `--config` which will display an interactive menu allowing you to select another version, or the command `--set` or `--set-selections` to make it without an interactive menu.

I really encourage you to experiment with this command if you need such feature, it will ease your workflow a lot.
You can also look at the [update-alternatives documentation](http://manpages.ubuntu.com/manpages/trusty/en/man8/update-alternatives.8.html) to know everything you can do with it, and be even more efficient 😉

----------

Thank you all for reading this article,
And until my next article, have a splendid day 😉

## Interesting links

- [Update-alternatives documentation](http://manpages.ubuntu.com/manpages/trusty/en/man8/update-alternatives.8.html)
- [GCC releases](https://gcc.gnu.org/releases.html)

