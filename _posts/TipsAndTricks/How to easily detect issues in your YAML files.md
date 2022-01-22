# How to easily detect issues in your YAML files

Hello ! I'm Xavier Jouvenot and in this small post, I am going to explain how to easily detect issues in your YAML files.

_Self promotion_: You can find other articles on computer science and programming on my [website](www.10xlearner.com) 😉

## Problematic

Whether you use YAML files to store server or CI/CI configuration, or simply data of your application, YAML is a language where a mistake can easily occurs, since indentation and spaces play a very important role in it.
Then, is can be very useful to have a tool which is able to check if the YAML files you are creating, using and modifying, in your daily work, are actually good, or if there are some major (or minor) issues in them.

## Solution

When you want to check if some source file contains correct code, the right thing to do is to look for something called a linter.
A linter is a software which flags programming errors, bugs, stylistic errors and/or suspicious constructs, in a file, or a full program, so that you can correct the last one accordingly and have a source file of better quality.

For YAML, there is actually a linter available, really easy to use, on all the main platforms, named [yamllint](https://github.com/adrienverge/yamllint).
You can use it directly in your terminal by using the following command:
```bash
yamllint <path/to/your/yaml/file.yml> <another/path/to/another/yaml/file.yml>
```

Once you will have run this command, you will have 3 possible results:
- An error, if you have given a bad file (`[Errno 2] No such file or directory: '[Errno 2] No such file or directory: 'aozdbaizd'`)
- The list of warnings and errors found by the program (for example: `4:4  error  wrong indentation: expected 4 but found 3 (indentation)`)
- Nothing is outputted, meaning that no error has been found in your file (also meaning that you are incredible at writing YAML 😉)

If you want to dive deeper in all the possibilities provided by this tool, I really encourage you, as there are several options that can help you adapt it to your specific needs (CI integration, warnings as error, configuration files, ...). For that purpose, you will find some links at the end of this article with to the [GitHub repository](https://github.com/adrienverge/yamllint) of the yamllint project, and to its [documentation](https://yamllint.readthedocs.io/en/stable/).

Moreover, if you want to know all there is to know about YAML, I really encourage you to look at this short [video (by TechWorld with Nana)](https://youtu.be/1uFVr15xDGg). This is the video that gave me the idea of this article, and it helped me to understand a lot more about YAML. 🙂

--------------

Thank you all for reading this article,
And until my next article, have a splendid day 😉

## Interesting links

- [GitHub repository with the actual working code up to date](https://github.com/Xav83/tutorials)
- [yamllint GitHub repository](https://github.com/adrienverge/yamllint), [documentation](https://yamllint.readthedocs.io/en/stable/) and [python package](https://pypi.org/project/yamllint/)
- [Learn YAML in 18 mins, by TechWorld with Nana](https://youtu.be/1uFVr15xDGg)
- [YAML website and specifications](https://yaml.org/)
- [Linter definition](https://en.wikipedia.org/wiki/Lint_(software))
- [YAML wikipedia page](https://en.wikipedia.org/wiki/YAML)
- [10xlearner website](www.10xlearner.com)
