# Formatting Python - Why and How !

Hello ! I'm Xavier Jouvenot and in this small post, I am going to explain why you should format you python scripts and how to do it.

_Self promotion_:
Here are a few social networks where you can follow me and check my work as a programmer and a writer 😉 
[personal website](www.10xlearner.com), [Twitter](https://twitter.com/10xLearner), [Dev.to](https://dev.to/10xlearner), [CodeNewbie](https://community.codenewbie.org/xav83), [Medium](https://medium.com/@xavier-jouvenot), [GitHub](https://github.com/Xav83/)

## Problematic (or why you should do it)

If you are not convinced that you should format you code, I really encourage you to go look at the previous article I did on [Formatting and Automation](https://10xlearner.com/2019/11/07/formatting-and-automation/), where I go in detail on why you should definitively format your code, and do it in a automatic way, to make your code more readable and make your team more productive since they won't have to spend time arguing about [Spaces VS Tabs](https://youtu.be/SsoOG6ZeyUI).

## Solution (or how to do it)

The short answer, for the people who don't want to read through the entire article (I know you do that! I do it too 😆) is to use the tool named [black](https://github.com/psf/black):

```bash
# Installation of the formatting tool on any platform
pip install black # with python3 !

#
black <path/to/python/script.py>
```

In the code above, we start by installing the tool [black](https://github.com/psf/black), which is described as *"The uncompromising code formatter"*.
I only showed you the simplest way to use this tool, by passing it, as arguments the list of the files you want to format, but there are a lot of other options available to customize the formatting process, check if the code is already formatted, for example.

You can find all the details in the [Black documentation](https://black.readthedocs.io/en/stable/) which is really complete and easy to read.😉

On important detail about this tool is that you can use it on a folder and it will recursively go though the sub-folders and format all the python files it will find. So, if you want, you can pass it the root directory containing all the files you want to format, and [black](https://github.com/psf/black) will do the rest 😉

--------------

Thank you all for reading this article,
And until my next article, have a splendid day 😉

## Interesting links

- [GitHub repository with the actual working code up to date](https://github.com/Xav83/tutorials)
- [GitHub repository of Black](https://github.com/psf/black)
- [Black tool documentation](https://black.readthedocs.io/en/stable/)
- [Black pip package](https://pypi.org/project/black/)
- [pip website](https://pypi.org/project/pip/)
- [10xlearner website](www.10xlearner.com), [Twitter](https://twitter.com/10xLearner), [Dev.to](https://dev.to/10xlearner), [CodeNewbie](https://community.codenewbie.org/xav83), [Medium](https://medium.com/@xavier-jouvenot), [GitHub](https://github.com/Xav83/)
