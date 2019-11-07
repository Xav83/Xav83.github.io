# My first open source contribution

Like a lot of person programming for living, I've always wondered how easy (or how hard, depending of your optimism level) it is to make open source contribution.
Moreover listening to a bunch of podcast on computer science, and programming, I've heard a lot contributors and maintainers talk about what contributions they made to open source, so I wanted to give it a try too.

## Why now ?

Why now, then, you may ask.
Well it a matter of opportunity.

Two weeks, when I developed the solution to a [Advent of Code problem](https://10xlearner.com/2019/09/23/177/) for my blog, I had faced a problem.
A tool that I use, [Cppcheck](http://cppcheck.sourceforge.net/), to add some static analysis checks on the code written of my solution on [Advent of Code](https://github.com/Xav83/AdventOfCode), gave me some warnings for code coming from a library I used (for information, this is the awesome library [JSON for Modern C++](https://github.com/nlohmann/json)).

At that point, I had several solution: I could either ignore them, or report them.
So I went to the GitHub repository of the library, I've created an [issue](https://github.com/nlohmann/json/issues/1759).
But looking at the warnings, I said to myself, why not fix some of them.
And here it is, I had a goal, I was ready to achieve it.

## From the decision to the contribution

In the open source world, a contribution is more than simple writing some code.
Indeed, you have to adapt the code you are going to produce to the guidelines of the open source project, and follow a specific process to do it.

Since the library [JSON for Modern C++](https://github.com/nlohmann/json) is on Github, I am going to describe the contribution process on GitHub.

### First step : the Fork

Ever wondered what the `Fork` button is useful for, on a repository interface ?
When you click on it, you will end up with a copy of the repository you are on, so that you can experiment with the code of the project, without affecting the original project repository.

This is the first step to make any contribution to a GitHub open source project.

So I did exactly that, I forked the repository of the library [JSON for Modern C++](https://github.com/nlohmann/json).

### Second step : the Commit

So now, I had a copy of the library repository, and started to apply some modifications on the code.
But I decided to start small.

Indeed, I've got 29 warnings from Cppcheck on the library, but I decided to act on only two of them : [an unecessary test](https://github.com/nlohmann/json/pull/1760/commits/b9dfdbe6be73bfa57e024ac45f7a184354f0f6f5) and [a use of the algorithm std::accumulate instead of a raw loop](https://github.com/nlohmann/json/pull/1760/commits/13a7c602578878bd524da551a1005b92e8d8e79b)

I think that I have never been so nervous writing code than when doing those two commits 😆
But it's good, it made me think a lot about the code I was writing.

### Third step : The Pull request

This step can be the more difficult one.
Indeed while you don't create a Pull Request, nobody will see you work.
But once you create a Pull Request, then it's out, and people can see and comment your work.

Creating the Pull Request is easy !
You push your branch, go on GitHub, click on the button proposed to create the Pull Request with your branch on the library repository.

The difficult part is to have it accepted by the maintainers.
Making it pass all the tests when they are a decent test suite is another difficulty too, but at least, you are more sure that you have broken the library.

What about the [Pull Request](https://github.com/nlohmann/json/pull/1760) I've created, you may ask.
Well, I had to make several fixes when I created it.
Indeed, in [JSON for Modern C++](https://github.com/nlohmann/json), you have to run and commit the result of their command `make amalgame`, which make several thing like formatting the code, and generate some files too.
After running this command, and push the modification, then, the tests stop passing, so I've fixed the code I've written, and make it follow the C++11 standard, since I've used a c++14 feature.
And after all those corrections, the tests finally passed again 🤘😂🤘
And finally, the Pull Request was approved and merged !

### Fourth Step : Enjoy and Celebrate

I found it important, every time I do something good, and something I want to do again, to celebrate.
By celebrating, I don't mean to trow a huge party just for washing the dishes, of course !
But to give myself a reward, which will help to do it again, so that your brain understand that something good just happen even if it wasn't easy and seemed like hard work.

## Conclusion

That was a good experience !
Interacting with another code base.
Seeing the tests sets up on it, and its workflows, I found that pretty interesting.

Moreover, I, now, am more familiar with the contribution process, which is good too.
I definitely do it again, and share my experience if I ever get some inside.
And writing about also help me to measure how much this experience gave to me.

Hopefully, this post will make you curious to try to contribute too, and until next post, have fun learning and growing 🙂
