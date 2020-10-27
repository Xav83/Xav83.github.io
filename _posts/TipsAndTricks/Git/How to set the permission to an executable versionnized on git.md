# How to set the permission to an executable versionnized on git


Hello ! I'm Xavier Jouvenot and in this small post, we are going to see how to set the permission to an executable file versionnized on git.

_Self promotion_: You can find other articles on my [website](www.10xlearner.com) :wink:

## Problematic

If you encountered this article, you may already be familiar with the problem, but I am going to explain it anyway :stuck_out_tongue:

When you want to commit an executable (a binary) into a git repository, you may encounter a problem when you check it out on another machine.
Indeed, an error can occur, more or less explicit about the real problem, forbidding you to run your versionnized program.

In fact, this problem comes from the permission rights on the binary that are being modified when committing and checking out the file.

## Solution

To solve this problem, you want to specify to git that you want to have the permission to execute the files that you want to commit.
By doing so, when you are going to check them out, you will be able to run the executable you want right away.

The command to achieve such a behavior is the following one:

```shell
git add --chmod=+x -- <folder_or_file>
git commit -m "An explicit commit message"
```

The first command stages the file or the folder that you want to add, with the right permission, to be able to run either the executable passed in, or the executables contained in the folder passed to the command.
The second command is a classic git command to create a commit.

With those two, your problem won't be one anymore :wink:

-------------

Thank you all for reading this article,
And until my next article, have a splendid day :wink:

## Interesting links

- [Stack overflow reference](https://stackoverflow.com/questions/21691202/how-to-create-file-execute-mode-permissions-in-git-on-windows)

