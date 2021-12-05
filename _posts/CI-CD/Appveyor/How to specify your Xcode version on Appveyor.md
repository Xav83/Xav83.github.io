# How to specify your Xcode version on Appveyor

Hello ! I'm Xavier Jouvenot and in this small post, I am going to explain how to specify your Xcode version on Appveyor.

_Self promotion_: You can find other articles on computer science and programming on my [website](www.10xlearner.com) 😉

## Problematic

If you have CI/CD processes or if you want to create some for your project, then it may be wise to make sure that you can always compile your project using the same version of Xcode as the one you are using on your machine, or the one used by your development team.

Another use case where specifying a version of Xcode can be very useful, is to upgrade the version you, and/or your dev team, are actually using.
Indeed, by specifying a newer version of Xcode in your CI/CD configuration, you can make sure everything works with the new version before asking everybody to upgrade the version of Xcode on their machine, and have a smooth transition.

## Solution

The short answer, for the people who don't want to read through the entire article (I know you do that! I do it too 😆) is to insert the following setup in your Appveyor process, before trying to compile anything:

```yml
build_script:
  - sudo xcode-select -s /Applications/Xcode-<version_number>.app
```

So, for example, if you want to select the version `13.1` of Xcode on Appveyor, then, it would look like that:

```yml
build_script:
  - sudo xcode-select -s /Applications/Xcode-13.1.app
```

If you are not sure about the version of Xcode you are actually using on Appveyor, you can always look to the [documentation](https://www.appveyor.com/docs/macos-images-software/#xcode), or you can add the following step to you process and see the information directly in Appveyor:

```yml
build_script:
  - sudo xcode-select -p
```

## Diving deeper with concrete examples

To make sure that, even in the future, you have working examples to look at, I have made a [GitHub repository](https://github.com/Xav83/tutorials) with an Appveyor process for each version of Xcode present on each OSX virtual environment available.

In the [appveyor.yml file](https://github.com/Xav83/tutorials/blob/main/.appveyor.yml) at the root of the repository, you will see jobs set up for each OSX environment using a [matrix](https://www.appveyor.com/blog/2018/04/25/specialized-build-matrix-configuration-in-appveyor/), running a [shell script](https://github.com/Xav83/tutorials/blob/main/Xcode%20version/run.sh) in a dedicated folder which selects each version of XCode available and print it once it has been selected.

![Xcode jobs depending on the environment](https://github.com/Xav83/Xav83.github.io/blob/master/res/Appveyor/Xcode_jobs.png)

![Xcode job selecting multiple versions](https://github.com/Xav83/Xav83.github.io/blob/master/res/Appveyor/Xcode_job.png)

I will maintain those Appveyor processes when Appveyor Systems Inc. will update their environment images, so that we will always be able to use this repository as reference! So if you see a version of Xcode or an version of OSX missing, do not hesitate to create a new issue 😉

--------------

Thank you all for reading this article,
And until my next article, have a splendid day 😉

## Interesting links

- [GitHub repository with the actual working code up to date](https://github.com/Xav83/tutorials)
- [Xcode releases](https://xcodereleases.com/)
- [Xcode versions available on Appveyor environments](https://www.appveyor.com/docs/macos-images-software/#xcode)
- [10xlearner website](www.10xlearner.com)
