# How to switch between 2 versions of Xcode installed on your computer

Hello ! I'm Xavier Jouvenot and in this small post, I am going to explain how to switch between 2 versions of Xcode installed on your computer.

_Self promotion_: You can find other articles on computer science and programming on my [website](www.10xlearner.com) 😉

## Problematic

Recently, I have been working on several projects using different versions of Xcode to be compiled.
Since those projects are using CMake to generate the Xcode solution and that I want to compile without leaving the terminal, I had to come up with a solution, and actually, I found out that there is actually a simple and elegant solution to this problem !

## Solution

The short answer, for the people who don't want to read through the entire article (I know you do that ! I do it too 😆) is using the following command:

```bash
sudo xcode-select -s <path/to/the/XCode/you/want/to/use>
```

So let's imagine that, in your `Applications` root folder you have 2 applications of Xcode for 2 different versions: `Xcode_12.4.app` and `Xcode_13.1.app`, then, all you have to do is:
```bash
sudo xcode-select -s /Applications/Xcode_12.4.app # Makes Xcode 12.4 you default Xcode
sudo xcode-select -s /Applications/Xcode_13.1.app # Makes Xcode 13.1 you default Xcode
```

After that, the `xcodebuild` you will be using will be the one of the Xcode you selected, and your CMake projects will use this version of Xcode too 🙂

And finally, if you want to know which Xcode you are currently using, all you have to do is to run the following command:

```bash
sudo xcode-select -p
```

--------------

Thank you all for reading this article,
And until my next article, have a splendid day 😉

## Interesting links

- [xcode-select documentation](https://www.manpagez.com/man/1/xcode-select/)
- [Xcode releases](https://xcodereleases.com/)
- [10xlearner website](www.10xlearner.com)
