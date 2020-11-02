# How to create a multi-platform job on Azure Pipelines

Hello ! I'm Xavier Jouvenot and in this small post, we are going to see how to create a multi-platform job on Azure Pipelines.

_Self promotion_: You can find other articles on my [website](www.10xlearner.com) 😉

## Problematic

When setting up your continuous integration and delivery environments on Azure Pipelines, or on any CI/CD system, you may want to use several environments/operating systems to test your program on.
Indeed, the more platform you can target, the more potentials client are available to you.

But, one program does not necessarily behave the same depending on the operating system.
Moreover, if, to generate you program on a specific environment, you need a compilation step which can also depend on the operating system.

## Solution

Depending of your needs, several solutions are available to you.

#### One job for one operating system

The first solution consists to specify the operating for each step/job on the Azure Pipelines process.
This solution is well adapted if, depending of the operating system, your script differs a lot.

```yml
jobs:
  - job: My_Job
    pool:
      - vmImage: '<operating_system>'
```

You can access to the list of all the operating system available [here](https://docs.microsoft.com/en-us/azure/devops/pipelines/agents/hosted?view=azure-devops&tabs=yaml#software).

With this solution, you will define the operating system for each of your Azure Pipelines job without problem.
But if the processes are the same (or very similar), you may want to look at the next solution available :wink:

#### Same process on all operating system

This solution is really handfull if you have some process in common over the different operating system you want to use.

It consists to says to Azure Pipelines to literally run the same job on several configurations.

```yml
jobs:
  - job: My_Job
    strategy:
      matrix:
        mac:
          imageName: 'macOS-10.15'
        linux_ubuntu_16_04:
          imageName: 'ubuntu-16.04'
        linux_ubuntu_18_04:
          imageName: 'ubuntu-18.04'
        linux_ubuntu_20_04:
          imageName: 'ubuntu-20.04'
        windows_vs_2017:
          imageName: 'vs2017-win2016'
        windows_vs_2019:
          imageName: 'windows-2019'
    pool:
      vmImage: $(imageName)
```

With such a configuration, your job will be duplicated on 6 environnements and run its own script on each of those environment.

This is very handy to avoid to duplication and can also be combined with the next point, if from one environment to the other, only a small modification is needed.

#### Checking the OS

In Azure Pipeline configuration file, you can specify some conditions specifying if a script or a job should be run or not ([Azure Pipelines condition documentation](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/conditions?view=azure-devops&tabs=yaml)).

Using those condition, you can also specify a step of a job to run only on a specific environment, like that:

```yml
- script: |
    echo "Hello World from OSX"
  displayName: Install dependencies
  condition: and(succeededOrFailed(), eq(variables['Agent.OS'], 'Darwin'))
```

Using [Agent.OS](https://docs.microsoft.com/en-us/azure/devops/pipelines/build/variables?view=azure-devops&tabs=yaml#agent-variables-devops-services) predefined variable, we get the current operating system and use it in a condition to make a step or a job to be triggered only on a specific environment.

And with that last technique, you are now able to run any job on whatever operating system that you want with Azure Pipelines.

--------------

Thank you all for reading this article,
And until my next article, have a splendid day 😉

## Interesting links

- [Microsoft-hosted agents](https://docs.microsoft.com/en-us/azure/devops/pipelines/agents/hosted?view=azure-devops&tabs=yaml)
- [Azure Pipelines documentation](https://docs.microsoft.com/en-us/azure/devops/pipelines/?view=azure-devops)
- [Azure Pipelines condition documentation](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/conditions?view=azure-devops&tabs=yaml)
- [10xlearner website](www.10xlearner.com)

