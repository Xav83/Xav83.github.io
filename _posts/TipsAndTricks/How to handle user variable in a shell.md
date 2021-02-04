# How to handle user variable in a shell/bash script

Hello ! I'm Xavier Jouvenot and in this small post, we are going to see how to handle user variable in a shell/bash script.

_Self promotion_: You can find other articles on my [website](www.10xlearner.com) 😉

## Problematic

When creating some shell scripts, you may want to have some parameters accessible to the user, so that he could either pass some inputs to your script, or specify some elements for which you will have set some default value.

There are several ways to do it, but we are going to see the simplest and most powerful solution to me.
Hopefully, you will find it convenient too 😉

## Solution

Let's dive right in the shell script and see what defining a user variable looks like:
```shell
[ -n "$VARIABLE" ] || VARIABLE=DEFAULT_VALUE; 

# Rest of the script ...
```

Even if it only take one line, a lot of things happen here. Let's take a little time to explain exactly what 🙂
In the first part of this line, `[ -n "$VARIABLE" ]`, we check if VARIABLE is *not empty*.
Then, with the operator `||` we indicate that we want to do something if the previous condition is false, which means we want to do something if VARIABLE is not *not empty*, or, if you remove the double negation for clarity, wa want to do something is the variable is empty.
And finally, we declare want we want to do in that case, which is to set VARIABLE to the default value that we specified. 😉

Now, all we need is to pass a value to the variable like that:

```shell
script.sh # uses the default value specified in the shell script for VARIABLE
script.sh VARIABLE= # set VARIABLE to an empty value
script.sh VARIABLE=SPECIFIC_VALUE # set VARIABLE to a SPECIFIC_VALUE
```

And voilà now, how to to handle user variable in a shell/bash script 😉

----------

Thank you all for reading this article,
And until my next article, have a splendid day 😉

## Interesting links

- [Documentation of if condition in bash](https://tldp.org/LDP/Bash-Beginners-Guide/html/sect_07_01.html)
