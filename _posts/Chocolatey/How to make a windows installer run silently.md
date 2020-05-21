# How to make a windows installer run silently

Hello ! I'm Xavier Jouvenot and in this small post, we are going to see how to make a windows installer run silently.

_Self promotion_: You can find other articles on my [website](www.10xlearner.com) 😉

## Options for installers ? 😮

Until I look on the internet about this subject (in order to create chocolatey packages), it never occured to me that you could pass arguments to an installer on Windows ! 😮

But you totally can, and depending on the installers, you have different parameters available to you, in order to specify some behavior of the installer.
I have put the link to the documentations of the installers that I know of at the end of the articles, if you want to know more about all the options they can give you.
If you know about another kind of installer, do not hesitate to share it with me, I will gladly add it in this article 🙂

## Silencing the installer

As I explained in the previous part, depending on the installer, the parameters are different.
This means that for silencing the installer, this option to pass to the installer is different from one installer to the other.
Here are the options silencing the installers:

- Microsoft Standard Installer - `/quiet`
- NSIS Installer - `/S`
- Inno Setup Installer - `/SILENT` or `/VERYSILENT`

--------------

Thank you all for reading this article,
And until my next article, have a splendid day 😉

## Interesting links

- [Microsoft - Standard Installer Command-Line Options](https://docs.microsoft.com/en-us/windows/win32/msi/standard-installer-command-line-options)
- [NSIS - Command Line Usage](https://nsis.sourceforge.io/Docs/Chapter3.html)
- [Inno Setup - Setup Command Line Parameters](https://jrsoftware.org/ishelp/index.php?topic=setupcmdline)
- [10xlearner website](www.10xlearner.com)

