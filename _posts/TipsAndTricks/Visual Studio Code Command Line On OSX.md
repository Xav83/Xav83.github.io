# How to set up Visual Studio Code command line on OSX ? - Quick Tip

Two week ago, I had to reinstall all the tools I use on a new computer, since the previous one passed away ☠️

And since I often use Visual Studio Code when developing, (please, no debate over which text editor is the best in the comment 😝), I had to reinstall it and make it easily accessible through the terminal.

## The command `code`

For those who don't know, Visual Studio Code has a shortcut, a command line tool named `code`, which allow you to use summon Visual Studio Code though your terminal.

Once setup up, you can use command line like `code my_file_or_folder` to open a file or a folder in Visual Studio Code. You can also add the option `-n` to open it in a new windows, or the option `-h` to display the help and have all the description of all the possibilities available to you. 😉

## Setting up `code` on OSX

The first step to have the command `code` accessible is to install Visual Studio Code, obviously.
If this is not already the case for you, then, follow this [link](https://code.visualstudio.com/download), download and install it on your computer.

At this point, you should be able to open Visual Studio Code, without using the terminal 😉

So now, let's focus on the main goal, being able to call the command `code` from your the terminal.
To do so, you must open Visual Studio Code. Then press the keys command-shift-P (or ⌘-⇧-P), a text field should appear, stating with a `>`. Now, type "Shell Command: Install 'code' command in PATH", and click on the option. Once installed (it shouldn't be long), you will have a notification poping up in the bottom right corner of Visual Studio Code.

All you have to do now, is to close your terminal and reopen it, and you will be able to run the command `code`. To make sure of it, you can run the command `code -v` to display your version of the command line installed 😉

## Conclusion

Personally, I needed to write this post so that future me will be able to go back to it whenever he will need to do so 😆
And maybe some of you, awesome people, will need it in the future too, so here it is 😉

Thank you all for reading this article !
And until my next article, have an splendid day 😉

## Other resources

- [Visual Studio Code Osx Documentation](https://code.visualstudio.com/docs/setup/mac)
