# AppVeyor and python formatting

Hello ! I'm Xavier Jouvenot and in this small post, I am going to explain how to automatically check if your python code is well formatted with AppVeyor.

_Self promotion_:
Here are a few social networks where you can follow me and check my work as a programmer and a writer 😉 
[personal website](www.10xlearner.com), [Twitter](https://twitter.com/10xLearner), [Dev.to](https://dev.to/10xlearner), [CodeNewbie](https://community.codenewbie.org/xav83), [Medium](https://medium.com/@xavier-jouvenot), [GitHub](https://github.com/Xav83/)

## Problematic

Today, we are going to focus on how we can automatically check if the python code you are versioning is well formatted, and more specifically, how you can use AppVeyor to do so. To automate this process will make your team more efficient and the code more consistent since everyone will have to be on board and format their code each time they want to contribute.

If you are not convinced that you should format you code, I really encourage you to go look at the previous article I did on [Formatting and Automation](https://10xlearner.com/2022/03/16/formatting-python-why-and-how), where I go in detail on why you should definitively format your code, and do it in a automatic way, to make your code more readable and make your team more productive since they won't have to spend time arguing about [Spaces VS Tabs](https://youtu.be/SsoOG6ZeyUI), for example.

## Solution

The short answer, for the people who don't want to read through the entire article (I know you do that! I do it too 😆) is to insert the following steps in your AppVeyor process:

```yml
install:
  - pip3 install black

build_script:
  - black --check .
```

If you have my previous article about ["Formatting Python - Why and How !"](https://10xlearner.com/2022/03/16/formatting-python-why-and-how), you may already know about about the tool named [black](https://github.com/psf/black). For those who don't know, this is a code formatter, described by its authors as follow:
> Black is the uncompromising Python code formatter. By using it, you agree to cede control over minutiae of hand-formatting. In return, Black gives you speed, determinism, and freedom from pycodestyle nagging about formatting. You will save time and mental energy for more important matters.

In the steps above, I start by installing this tool with the [python package manager pip](https://pypi.org/project/pip/). Then, I run `black` at the root of the folder, meaning that it is going to find all the Python files in repository and, since we are using the flag `--check`, verify if they are well formatted. If one Python file is not formatted correctly, then, the black command will return an error, and the corresponding step in AppVeyor will fail.

If you want, you can specify directly the list of the Python files, instead of letting the tool scan through all the repository recursively. 😉

Finally, it is **important to specify** that those steps work only on a majority of the environments available in AppVeyor.
Indeed, [black](https://github.com/psf/black) only works with the version [Python 3.6 and above](https://black.readthedocs.io/en/stable/faq.html#which-python-versions-does-black-support) which is not the default version on some AppVeyor environments. On Windows and Ubuntu environments, you will have to first update the default version of Python used by the environment before being able to run black on your Python code. You can take a look at this [script](https://github.com/Xav83/tutorials/blob/main/Python%20format/run.sh), to see how I have done it when working on this article 😊

I actually made a GitHub repository in which I setup AppVeyor to run the steps described before on every environment available. You can take a look [here](https://github.com/Xav83/tutorials/blob/main/.appveyor.yml#L51) as I will update it when new environment will be available on AppVeyor 😉

--------------

Thank you all for reading this article,
And until my next article, have a splendid day 😉

## Interesting links

- [GitHub repository with the actual working code up to date](https://github.com/Xav83/tutorials), and the [Azure Pipelines jobs runned](https://ci.appveyor.com/project/Xav83/tutorials)
- AppVeyor [Windows](https://www.appveyor.com/docs/windows-images-software/), [Linux](https://www.appveyor.com/docs/linux-images-software/), and [MacOs](https://www.appveyor.com/docs/macos-images-software/) environments
- [black GitHub repository](https://github.com/psf/black), [documentation](https://black.readthedocs.io/en/stable/) and [python package](https://pypi.org/project/black/)
- [Python website](https://www.python.org/)
- [pip website](https://pypi.org/project/pip/)
- [10xlearner website](www.10xlearner.com), [Twitter](https://twitter.com/10xLearner), [Dev.to](https://dev.to/10xlearner), [CodeNewbie](https://community.codenewbie.org/xav83), [Medium](https://medium.com/@xavier-jouvenot), [GitHub](https://github.com/Xav83/)
