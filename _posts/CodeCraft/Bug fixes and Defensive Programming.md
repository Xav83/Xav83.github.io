# Bug fixes and Defensive Programming

Hi dear reader, I'm Xavier Jouvenot and in this article, we are going to talk about we should add assertion each time we correct a bug. This post was inspired by a rule from the first chapter of [Code Craft, by Pete Goodliffe](https://amzn.to/2ZrTaHQ), on [Defensive programming](https://10xlearner.com/2020/01/06/defensive-programming-code-craft/).

## Small Recap on Defensive Programming

For those who have not read my [article on Defensive programming](https://10xlearner.com/2020/01/06/defensive-programming-code-craft/).
First of all, "BOUH This is BAAAD ! You should read IT !"

Just kidding, do whatever you want ! You are awesome ! 😉👍

More seriously, the point of [Defensive programming](https://10xlearner.com/2020/01/06/defensive-programming-code-craft/) is to prevent bugs, by adopting some rules, habits which will allow programmers to work better as a team and be more efficient. Those rules can be ["Use Good Coding Style"](https://10xlearner.com/2019/12/05/formatting-cpp-c-javascript-and-other-stuff/), ["Name Correctly your variables"](https://10xlearner.com/2020/01/21/the-power-of-naming-code-craft/), or use pre-condition and post-condition as much as possible.

## Prevent bugs, so no bug to fix, right ? 🤔

In a perfect world, where you could prevent all bugs, you would have bug to fix.

BUT, in the real world, our world, nobody can prevent all the cases, even if you are the best in your field, there is always something you don't know about the system you work on, or the code base, or the user system, ... so you will have bugs to fix. Defensive programming allows you to prevent a lot of bugs, so you should you it, and only have to fix the ones left, that pass through your defenses.

## Building your defense along the way

Preventing bugs isn't over when you finish building a feature, it should continue when you are in production, and maintaining a software.
So you can apply [Defensive programming](https://10xlearner.com/2020/01/06/defensive-programming-code-craft/) philosophy on the process of fixing bugs.

Indeed, once you fixed you bugs, you know more about the component causing the bugs than before (or the components, if the origin of the bug trigger a chain reaction). So, now, you can add guard in your code to avoid the same bug, or class of bugs to trigger new problems, and make your program more robust for a better future. 😉

You can do that by adding new assertions in your code to make sure that the state of the bugged component won't happen again, or that you will be notify if it does.

## Test your defenses

Correcting a bug and adding more guards in your code is really a good step to have a more robust program.
You can go further though, by using [Offensive programming](https://en.wikipedia.org/wiki/Offensive_programming). ⚔️

Indeed, battle testing your program by writing tests that check if your code behave correctly in a maximum of conditions, and checking that your guard hold well, is a huge plus in software development and maintenance.

In most languages, there are testing libraries that allow you to write your own test and to make sure that your program will resist the situation it will face. It will also allow you to reproduce the situation where your software fails, so that you can fix it correctly. So there is no reason not to do it. 😉

## Conclusion

Bug fixing and Defensive Programming can seem to be very different process, but once you look carefully at it, you realize that you should be defensive all the time.

Not only adding guard make sure that the bug won't happen, but it reinforce the part of the code for its future uses and make it more clearer about what it expect as input and what does and give out as output.

Bad programmer only fix bugs. Good programmer make sure that they won't happen again. 😉

Thank you all for reading this article !
And until my next article, have an splendid day 😉
