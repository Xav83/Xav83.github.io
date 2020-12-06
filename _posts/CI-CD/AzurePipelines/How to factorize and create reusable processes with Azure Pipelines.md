# How to factorize and create reusable processes with Azure Pipelines

Hello ! I'm Xavier Jouvenot and in this small post, we are going to see how to factorize and create reusable processes with Azure Pipelines.

_Self promotion_: You can find other articles on my [website](www.10xlearner.com) 😉

## Problematic

When creating some processes on Azure Pipelines, you may want to simplify it like you do with code when using functions.
Indeed, it can be very helpful, if you want to share your processes to other people, to be able to make them generic enough so that other people could use them, or that, even in your own company, or in your own projects, to have one process with some parameters to set up to have a working Azure Pipeline process.

Sadly, there are no functions in Azure Pipelines, **but** there is a solution, or this blog post would not exist 😆.

## Solution

The solution for our problem is called a template.
It consists of a file describing a process where we can define some parameters.
This template can then be called from the main azure pipeline configuration file with some parameter's value to launch the process.

The template file look like that:

```yml
parameters:
    - name: configuration
      type: string
      default: Debug

steps:
    - script: |
        make run CONFIGURATION=${{ parameters.configuration }}
      displayName: Generate, Build and Run the solution
```

In this file, we start by defining the parameters possible and we define the process with those parameters, and the instruction we want to be executed.

Once the process defined, we can use this template in the main configuration file:

```yml
-jobs:
    - job: Job_Name
      steps:
        - template: .azurePipelines/my_template.yml
          parameters:
            configuration: Release
```

In this file, all we have to do to use a template file, is to specify where the template file is and the value for the parameters.

The two files in this article are functional but, I encourage you to check the [Azure Pipelines template documentation](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/templates?view=azure-devops) to se all the things you can do with template 😉

-----------

Thank you all for reading this article,
And until my next article, have a splendid day. 😉

## Interesting links

-  [Azure Pipelines template documentation](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/templates?view=azure-devops)
