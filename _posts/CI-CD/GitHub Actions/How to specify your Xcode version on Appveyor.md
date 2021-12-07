# How to specify your Xcode version on GitHub Actions

Hello ! I'm Xavier Jouvenot and in this small post, I am going to explain how to specify your Xcode version on GitHub Actions.

_Self promotion_: You can find other articles on computer science and programming on my [website](www.10xlearner.com) 😉

## Problematic

If you have CI/CD processes or if you want to create some for your project, then it may be wise to make sure that you can always compile your project using the same version of Xcode as the one you are using on your machine, or the one used by your development team.

Another use case where specifying a version of Xcode can be very useful, is to upgrade the version you, and/or your dev team, are actually using.
Indeed, by specifying a newer version of Xcode in your CI/CD configuration, you can make sure everything works with the new version before asking everybody to upgrade the version of Xcode on their machine, and have a smooth transition.

## Solution

The short answer, for the people who don't want to read through the entire article (I know you do that! I do it too 😆) is to insert the following setup in your GitHub Actions process, before trying to compile anything:

```yml
jobs:
  build:
    runs-on: macos-latest
    steps:
    - uses: maxim-lobanov/setup-xcode@v1
      with:
        xcode-version: 'version_number'
```

So, for example, if you want to select the version `13.1` of Xcode on GitHub Actions, then, it would look like that:

```yml
jobs:
  build:
    runs-on: macos-latest
    steps:
    - uses: maxim-lobanov/setup-xcode@v1
      with:
        xcode-version: '13.1'
```

This solution uses a free GitHub Actions, under the license MIT, named "setup-xcode" and you can find all the details about it on the [GitHub MarketPlace](https://github.com/marketplace/actions/setup-xcode-version).

If you are not sure about the version of Xcode you are actually using on GitHub Actions, you can always look to the documentation relative to the image you are using ([osx11](https://github.com/actions/virtual-environments/blob/main/images/macos/macos-11-Readme.md#xcode) or [osx10.15](https://github.com/actions/virtual-environments/blob/main/images/macos/macos-10.15-Readme.md#xcode)), or you can add the following step to you process and see the information directly in Azure Pipelines:

```yml
jobs:
  build:
    runs-on: macos-latest
    steps:
    - name: "Dsiplays Xcode current version"
      run: sudo xcode-select -p
```

## Diving deeper with concrete examples

To make sure that, even in the future, you have working examples to look at, I have made a [GitHub repository](https://github.com/Xav83/tutorials) with an GitHub Actions process for each version of Xcode present on each OSX virtual environment available.

In the [xcodes.yml file](https://github.com/Xav83/tutorials/blob/main/.github/workflows/xcodes.yml) in the GitHub Action folder (`.github/workflows`) of the repository, you will see jobs set up for each OSX environment using a [matrix](https://www.GitHub Actions.com/blog/2018/04/25/specialized-build-matrix-configuration-in-GitHub Actions/), and ["setup-xcode" action](https://github.com/marketplace/actions/setup-xcode-version) which selects a version of XCode available defined in the matrix.

![Xcode jobs depending on the environment](https://github.com/Xav83/Xav83.github.io/blob/master/res/GitHub%20Actions/Xcode_jobs.png)

![Xcode job selecting multiple versions](https://github.com/Xav83/Xav83.github.io/blob/master/res/GitHub%20Actions/Xcode_job.png)

I will maintain those GitHub Actions processes when GitHub will update their environment images, so that we will always be able to use this repository as reference! So if you see a version of Xcode or an version of OSX missing, or a way to improve the GitHub Actions script, do not hesitate to create a new issue 😉

--------------

Thank you all for reading this article,
And until my next article, have a splendid day 😉

## Interesting links

- [GitHub repository with the actual working code up to date](https://github.com/Xav83/tutorials)
- [Xcode releases](https://xcodereleases.com/)
- ["setup-xcode" GitHub Action](https://github.com/marketplace/actions/setup-xcode-version)
- [GitHub Actions Microsoft-hosted agents](https://github.com/actions/virtual-environments#available-environments)
    - [Xcode version for OSX 11](https://github.com/actions/virtual-environments/blob/main/images/macos/macos-11-Readme.md#xcode)
    - [Xcode version for OSX 10.15](https://github.com/actions/virtual-environments/blob/main/images/macos/macos-10.15-Readme.md#xcode)
- [10xlearner website](www.10xlearner.com)
