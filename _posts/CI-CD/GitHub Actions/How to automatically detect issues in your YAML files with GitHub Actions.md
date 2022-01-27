# How to automatically detect issues in your YAML file with GitHub Actions

Hello ! I'm Xavier Jouvenot and in this small post, I am going to explain how to automatically detect issues in your YAML file with GitHub Actions.

_Self promotion_: You can find other articles on computer science and programming on my [website](www.10xlearner.com) 😉

## Problematic

If you have CI/CD processes or if you want to create some for your project, and you have some YAML files versioned, then it may be important to make sure that they are always properly written and don't contain any error due to indentation mistake, for example.

Moreover, having an automatic process to approve the YAML will enhance the quality of your project and facilitate the contribution from other developers from your team, or even external collaborators, since they would have direct information from the CI about the issues in the YAML files.

## Solution

The short answer, for the people who don't want to read through the entire article (I know you do that! I do it too 😆) is to insert the following steps in your GitHub Action process:

```yml
steps:
  - name: Installs the latest version of Yamllint
    run: pip install yamllint
  - name: Runs yamllint on all the yaml file of the repository
    run: yamllint --strict .
```

If you have my previous article about ["How to easily detect issues in your YAML files"](https://10xlearner.com/2022/02/02/how-to-easily-detect-issues-in-your-yaml-files), you may already know about about the tool named [yamllint](https://github.com/adrienverge/yamllint). For those who don't know, this is a linter, a software able to analyse YAML files and displays the issues it found in them.

In the steps above, I start by installing this tool with the [python package manager pip](https://pypi.org/project/pip/). Then, I run `yamllint` at the root of the folder, meaning that it is going to find all the YAML files in repository and analyses them all. Moreover, by using the flag `--strict`, even the warnings found by the tool will be treated as errors, making the check fail, and notifying use that the commit has some issues.

If you want, you can specify directly the list of the YAML file, instead of letting the tool scan through all the repository recursively. 😉

Finally, it is interesting to notice that those steps can be uses on any environment available in GitHub Actions.
I actually made a GitHub repository in which I setup GitHub Actions to run the steps described before on every environment available. You can take a look [here](https://github.com/Xav83/tutorials/blob/main/.github/workflows/yamllint.yml) as I will update it when new environment will be available on GitHub Actions 😉

--------------

Thank you all for reading this article,
And until my next article, have a splendid day 😉

## Interesting links

- [GitHub repository with the actual working code up to date](https://github.com/Xav83/tutorials) and the [GitHub Actions worklows runned](https://github.com/Xav83/tutorials/actions)
- [GitHub Actions build worker images](https://github.com/actions/virtual-environments#available-environments)
- [yamllint GitHub repository](https://github.com/adrienverge/yamllint), [documentation](https://yamllint.readthedocs.io/en/stable/) and [python package](https://pypi.org/project/yamllint/)
- [Learn YAML in 18 mins, by TechWorld with Nana](https://youtu.be/1uFVr15xDGg)
- [YAML website and specifications](https://yaml.org/)
- [Linter definition](https://en.wikipedia.org/wiki/Lint_(software))
- [YAML wikipedia page](https://en.wikipedia.org/wiki/YAML)
- [pip website](https://pypi.org/project/pip/)
- [10xlearner website](www.10xlearner.com)
