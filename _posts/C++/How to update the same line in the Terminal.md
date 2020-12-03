# How to update the same line in the Terminal

Hello ! I'm Xavier Jouvenot and in this small post, we are going to see how to update the same line in the Terminal.

_Self promotion_: You can find other articles on my [website](www.10xlearner.com) 😉

## Problematic

When displaying information in the terminal with your program, you can easily end up spamming it with so much information that no human can understand what is happening. Moreover, you can also fill the history buffer, so that the useful information is lost if a user want to come back and look at what happened.

To reduce the amount of information displayed in the terminal, a solution is to update the same line over and over for similar informations.
For example, to mark the progress of a download/update, you don't have to display a new line each time a new bytes has been downloaded. You rather want to update the number of Bytes downloaded on the same line during the progression of the download, which will reduce the amount of flooding of your application.

Of course, there are also some cases where you want to display a lot of information in the terminal so that the user can look and search into all the information you can give to him.
So you should be well aware about which information you want to be displayed for later reading, and which one are only displayed and useful on the moment, during the execution of the program.

## Solution

Now that we have seen why and when it is important to be able to update the same line over and over in the terminal, let's the solution.

The solution is actually pretty short as you only need to use the character `\r` at the beginning of the string of characters that you want to display on the terminal.
By doing so, you will update the last line displayed in the terminal by replacing it with the one that you specified.
But be careful! If you end your last line with a new line character (`\n`), the `\r` will have no effect and a new line will be added in the terminal.

Let's look at an example:

```c++
std::cout << "Progress 0%";
std::this_thread::sleep_for(std::chrono::seconds(1));

std::cout << "\rProgress 10%";
std::this_thread::sleep_for(std::chrono::seconds(1));

std::cout << "\rProgress 20%";
```

This code will display "Progress 0%", then, one second later, the previous line will be replaced by "Progress 10%" and finally, another second later, the line will be updated once again with the string "Progress 20%".

And just like that, we have updated the last line in our terminal 🙂

----------

Thank you all for reading this article,
And until my next article, have a splendid day 😉

