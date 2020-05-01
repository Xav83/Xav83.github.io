# Quick Tip - How to display the history of a file using git ?

Hello ! I'm Xavier Jouvenot and in this small post, I am going to explain how to display the history of a file using git.

_Self promotion_: You can find other articles on Android development on my [website](www.10xlearner.com) 😉

## Git log

You may be familiar or not with this the command `git log` which can display the history of the commit of your repository, when you use it at the root of your repository.
But there is more about it !

Indeed, you can look to the [documentation](https://git-scm.com/docs/git-log) to see all the options available, and we are going to take a look at some of them.

For the problem targeted on this post, here is the command line solving it:
```shell
git log <path/to/your/file>
```

This command line will display your the details of each commit when the file has been modified.

If you want to only display the minimum of information to know retrieve the commit, you can add the flag `oneline` such as:
```shell
git log --oneline <path/to/your/file>
```

And if, on the contrary, you want to display more information such as the diff about the file involved in each of those commits, you can add the flag `-p`, like so:
```shell
git log -p <path/to/your/file>
```

--------------

Thank you all for reading this article,
And until my next article, have a splendid day 😉

## Interesting links

- [git log documentation](https://git-scm.com/docs/git-log)
- [10xlearner website](www.10xlearner.com)
