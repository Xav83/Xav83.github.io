# How to set up Sublime Text command line on OSX ? - Quick Tip

Last week, I had to reinstall all the tools I use on a new computer, since the previous one passed away ☠️

And since I use Sublime Text as a default text editor, (please, no debate over which text editor is the best in the comment 😝), I had to reinstall it and make it easily accessible through the terminal.

## The command `subl`

For those who don't know, Sublime Text has a shortcut, a command line tool named `subl`, which allow you to use summon sublime text though your terminal.

Once setup up, you can use command line like `subl my_file_or_folder` to open a file or a folder in Sublime Text. You can also add the option `-n` to open it in a new windows, or the option `-h` to display the help and have all the description of all the possibilities available to you. 😉

## Setting up `subl` on OSX

The first step to have the command `subl` accessible is to install Sublime Text, obviously.
If this is not already the case for you, then, follow this [link](https://www.sublimetext.com/3), download and install it on your computer. 

At this point, you should be able to open Sublime Text without the terminal 😉

So now, let's focus on the main goal, being able to call the command `subl` from your the terminal.
To do so, you must open the file `.bash_profile` which is normally in you `HOME` folder. To to so, you can type the command `open ~/.bash_profile`, for example.
If the file doesn't exist, it is okay, we can create an empty one where we will add our instruction. 🙂

Once this file open, add the following line wherever you want (if you have a complex `.bash_profile`, you may prefer put this line at the end of the file):
```bash
export PATH=$PATH:/Applications/Sublime\ Text.app/Contents/SharedSupport/bin
```

If you haven't installed Sublime Text in you Applications folder, change the path accordingly to the location where you put it, and tell me why you did so in the comments, I am curious 😃

Now close your opened terminal and reopen them, or simply run the command `source ~./bash_profile`, and you will be able to run the command `subl`. To make sure of it, you can run the command `subl -v` to display your version of the command line installed 😉

## Conclusion

If you are used to install software through script, this manipulation is probably trivial for you.
Personally, I needed to write this post so that future me will be able to go back to it whenever he will need to do so 😆

And maybe some of you, awesome people, will need it in the future too 😉

Thank you all for reading this article !
And until my next article, have an splendid day 😉

## Other resources

- [Sublime Text 3 Osx Documentation](https://www.sublimetext.com/docs/3/osx_command_line.html)