# Making my Advent Of Code solution cross-platform (a developer journey)

Hello ! I'm Xavier Jouvenot and today, I want to reflect a little bit on the solution to [Advent Of Code 2021](https://adventofcode.com) challenges I have done so far, and more precisely on how and why I have made my solution cross-platform.

_Self promotion_:
Here are a few social networks where you can follow me and check my work as a programmer and a writer 😉 
[personal website](www.10xlearner.com), [Twitter](https://twitter.com/10xLearner), [Dev.to](https://dev.to/10xlearner), [CodeNewbie](https://community.codenewbie.org/xav83), [Medium](https://medium.com/@xavier-jouvenot), [GitHub](https://github.com/Xav83/)

## My Advent of Code journey

Most of you, who are reading this article may not know, but my first encounter with the Advent Of Code challenges actually matches the time when I have created my first personal website ([10xlearner.com](www.10xlearner.com)) and my [first post](https://10xlearner.com/2019/06/03/advent-of-code-not-quite-lisp-puzzle-1/) was about how I came up with a solution for the first problem of Advent Of Code 2015. Even if it was in 2019, I wanted to start the very first challenge ever propose by Advent Of Code.

I end up creating 18 posts, one for the 18th first problem of the Advent Of Code 2015, and then, I went on other projects, write some other articles, stop for a moment, write again, stop again, ... well you get the point 😄.

2 years and 7 months later, I posted a new post about my solution to the first challenge of Advent Of Code 2021. My goal was (and still is) to write one article, each week about how I found the solution to a day problem of Advent of Code 2021. We are now at 10 days solved (when I am writing this post).
And I have to say that it is a very interesting experience for me, challenging my mind, in order to come up with a solution with which I am satisfied about.

Moreover, I have interacted a little bit with the [Advent Of Code reddit](https://www.reddit.com/r/adventofcode/), and I add some really good and interesting feedback about how I could improve my solutions to the different problems and how I could make my post more interesting, more entertaining. With all that feedback, I definitively made some progress ! I have tried different things to make my content more engaging, so that you, dear readers, take pleasure into reading what I write. Even my approach to the problems has changed a little bit which is a good thing (well I hope it is).

Still, one of my main objective when coming up with a solution to a problem, is to make it as simple as possible in order to allow anyone to understand the logic used to find the solution, even if they are not fluent in C++. I often find people thinking that C++ is an elite language, difficult to apprehend, understand or use and I want to show to people that this is not the case. Like all programming languages, you can write code that is very difficult to read, or you can write really expressive code for people to understand. 

With those goal in mind, I am going to continue my journey in Advent of Code problems, experimenting different solutions to solve them and improving during the process as a programmer and as a writer. 😊

## Why a cross-platform goal for such challenge

If you have followed me on social medias, or looked at my past articles, you will see that I am really interested on some subjects like code quality, software tools, automation and DevOps in general. And when writing the code for the different problem of Advent Of Code, I was totally satisfied by the code in itself, as I know that there is room for improvement. So I decided to improve it.

The first step to improving the code I produce for those solution was to setup a CI process, to make sure that the code will always compile, whatever the modification I will do in the future. To do so, I have used Azure Pipelines (because I had to choose one), and I have created a configuration allowing me to make sure that my code can be compiled on the 3 main Operating Systems: Windows, OSX and Linux.

There are going some other improvements in the future, like adding some linters/static analysis tools, using different compilers, adding some tests in the project. But having a CI seemed to me like the first step to take in order to have automatic feedback on the code I am writing and a solid base to, then, integrate all the improvements I have mentioned. 😊

## How did I to achieve cross-platform compatibility

As I said in the previous part, using Azure Pipelines, I am able to make sure that my code compiles on the main OS on the market. The first thing you must know is that, I use [CMake](https://cmake.org/) in order to generate a compilation process which is standard on any platform. Indeed, I can compile my code with the following commands, on any OS:
```bash
# in a build folder
cmake .. -G "<generator_to_specify>" # Generates the solution to compile
cmake --build . # Compiles the solution generated previously
```

From that process, I was able to create a simple template for Azure Pipelines describing the compilation process. You can find it is the file [.azure-pipeline/build.yml](https://github.com/Xav83/adventofcode2021/blob/main/.azure-pipeline/build.yml) in my GitHub repository. It looks like that:
```yml
parameters:
  - name: 'generator'
    type: string
  - name: 'root_directory'
    type: string

steps:
  - script: |
      cd "${{ parameters.root_directory }}"
      mkdir build
    displayName: Creates build folder
  - script: |
      cd "${{ parameters.root_directory }}/build"
      cmake .. -G "${{ parameters.generator }}"
    displayName: Generates the solution
  - script: |
      cd "${{ parameters.root_directory }}/build"
      cmake --build .
    displayName: Compiles the solution
```

Finally, all I have left to do, is to use this template on different OS provided by Azure Pipelines. You can find it in the [.azure-pipelines.yml file](https://github.com/Xav83/adventofcode2021/blob/main/.azure-pipeline.yml) in my GitHub repository. It looks like:
```yml
parameters:
  - name: days
    type: object
    default: ['Day 1', 'Day 2', 'Day 3', 'Day 4', 'Day 5', 'Day 6', 'Day 7', 'Day 8', 'Day 9', 'Day 10']

jobs:
  - job: Advent_Of_Code
    strategy:
      matrix:
        visual_studio_2019:
          imageName: 'windows-2019'
          generator: Visual Studio 16 2019
        visual_studio_2022:
          imageName: 'windows-2022'
          generator: Visual Studio 17 2022
        xcode:
          imageName: 'macOS-11'
          generator: Xcode
        unix_makefiles:
          imageName: 'ubuntu-20.04'
          generator: Unix Makefiles
    pool:
      vmImage: $(imageName)
    steps:
      - ${{ each day in parameters.days }}:
          - template: .azure-pipeline/build.yml
            parameters:
              generator: $(generator)
              root_directory: ${{ day }}
```

The parameter `days` allows me to specify which days' solution I want to compile, as I have a folder dedicated to each day's solution.
Then, the `matrix` and the `pool` makes Azure Pipelines create 4 jobs: 2 on Windows (with different version of Visual Studio), 1 on OSX (with Xcode) and 1 on Linux (with Unix Makefiles as a generator).
And finally, the instruction `${{ each day in parameters.days }}` allows me to specify the template `.azure-pipeline/build.yml` for each day and each generator defined before.

![Azure Pipeline jobs](https://raw.githubusercontent.com/Xav83/Xav83.github.io/master/res/Advent%20Of%20Code/2021/Screenshot%20Of%20Azure%20Pipeline%20CrossPlatform.png)

## The benefits of cross-platform and CI

When I have set up Azure Pipelines, I have solved 5 days of Advent Of Code 2021.
But setting Azure Pipelines was all I had to do ! Indeed, the code I have written wasn't cross-platform, it actually only compiled on Linux, with the generator "Unix Makefiles", so I had to start fixing the different issues. Those issues come from difference in the implementation of different compilers and I had to adapt a little bit. There were actually only 3 kind of problems:
- [Commit 0784ae](https://github.com/Xav83/adventofcode2021/commit/0784ae72195f3ab732c0adf3dadb6802be4a8bcd): in C++, there are, what is called, [alternative keywords](https://en.cppreference.com/w/cpp/language/operator_alternative) (`and` to replace `&&`, `or` to replace `||`, ...), and Visual Studio required you to do some configuration in order to be able to use them. So by adding the option `/permissive-` when compiling with Visual Studio, I was able to correct this issue.
- [Commit d91247](https://github.com/Xav83/adventofcode2021/commit/d91247331d10baca9d00591cdaab22df94e60e86): the generator `Unix Makefiles` can apparently do some magic ! Even if I didn't include the header file for the `vector` container, it was able to still compile, while the other generators (Xcode and Visual Studio) didn't. Some small include instruction did the trick 🧙 
- [Commit 9479d4](https://github.com/Xav83/adventofcode2021/commit/9479d437bb8796ba88914db4b67879c2ba42ffeb): one function that I marked with the keyword `constexpr` actually should not been able to have this `keyword`. Indeed, since it uses the function [std::abs](https://en.cppreference.com/w/cpp/numeric/math/abs) which is not `constexpr` in the standard documentation of C++, then, my function can have this keyword. So by removing this keyword, I was able to correct the issue and have cross-platform solution for all the first 5 days 😊

Once the CI set up, all I had to do was to update it when I wrote a new solution for a new day's problem. This required only one change in the configuration file of Azure Pipelines, and it was to update this line:
```yml
    default: ['Day 1', 'Day 2', 'Day 3', 'Day 4', 'Day 5', 'Day 6', 'Day 7', 'Day 8', 'Day 9', 'Day 10']
```
To add the new day solved

## Conclusion

As a conclusion, I can say that Advent Of Code 2021 was and still is a fun experience for me.
I will continue solving them and improving myself in the process. 😊

If you want to see the solutions of Advent Of Code 2021 from end to end and/or the configuration of Azure Pipelines, you can go on my [GitHub account](https://github.com/Xav83/adventofcode2021), explore the full solution, add comments or ask questions if you want to, on the platform you read this article, it will also help me improve the quality of my articles. 😉

--------------

Thank you all for reading this article,
And until my next article, have a splendid day 😉

## Interesting links

- [constexpr documentation](https://en.cppreference.com/w/cpp/language/constexpr)
- [std::abs documentation](https://en.cppreference.com/w/cpp/numeric/math/abs)
- [Alternative C++ keywords](https://en.cppreference.com/w/cpp/language/operator_alternative)
- [Azure Pipelines Microsoft agents](https://docs.microsoft.com/fr-fr/azure/devops/pipelines/agents/hosted?view=azure-devops&tabs=yaml)
- [CMake](https://cmake.org/)
- [Advent of Code](https://adventofcode.com/2021)
- [10xlearner website](www.10xlearner.com), [Twitter](https://twitter.com/10xLearner), [Dev.to](https://dev.to/10xlearner), [CodeNewbie](https://community.codenewbie.org/xav83), [Medium](https://medium.com/@xavier-jouvenot), [GitHub](https://github.com/Xav83/)

