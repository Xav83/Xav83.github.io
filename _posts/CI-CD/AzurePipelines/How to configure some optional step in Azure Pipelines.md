# How to configure some optional steps in Azure Pipelines

Hello ! I'm Xavier Jouvenot and in this small post, we are going to see how to configure some optional steps in Azure Pipelines.

_Self promotion_: You can find other articles on my [website](www.10xlearner.com) 😉

## Problematic

When creating a CI/CD pipeline, you may want to trigger some scripts only if some conditions are met.
Indeed, depending on the operating system, or on some inputs, you may want to have a pipeline able to adapt itself and run the right scripts to achieve your processes/tests.

I covered the operating system part of it in [another blogpost](https://10xlearner.com/2020/11/11/how-to-create-a-multi-platform-job-on-azure-pipelines/), but we are going to dive a little bit deeper in this article.

## Solution

In your Azure Pipeline configuration file, there is a very convenient keyword to do just that : `condition`.
By using this keyword, we will be able to specify... welll, some conditions, in which the step, or job targeted will be executed.

For example, if we want a script to only be executed if the operating system is a Mac environnement, the step will look like that:

```yml
- script: |
    echo "Hello World from OSX"
  displayName: OSX Job
  condition: and(succeededOrFailed(), eq(variables['Agent.OS'], 'Darwin'))
```

With `eq(variables['Agent.OS'], 'Darwin')`, we check the current operating system, and with `succeededOrFailed()`, we make sure that this `step` will run even if a previous dependency has failed, unless the run was canceled.

If you want to trigger a step of you pipeline depending on the parameters of a template, for example, you can do it like that

```yml
- script: |
    echo "Hello World"
  displayName: If parameter is release
  condition: eq('${{ parameters.configuration }}', 'Release')
```

With `configuration` being one of your parameters. In that case, this job will only be triggered if the `configuration` parameter is set to the value `Release`.

Finally, let see another use case, where you want a job to be executed or not, depending of the final state of another job:

```yml
- job: Bar
  dependsOn: Foo
  condition: failed()
  // Rest of the pipeline
```

In this example, the job `Bar` will only be run if the job `Foo` ended up with a failure.

These are some example of the use case I commonly encountered, so I hope that it will help you in your own pipelines.
I also encourage you to look into the [Azure Pipeline documentation](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/conditions?view=azure-devops&tabs=yaml), where you will find a more complete description on all the possibilities available to you 😉

-----------

Thank you all for reading this article,
And until my next article, have a splendid day 🙂

## Interesting links

- [Azure Pipeline condition documentation](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/conditions?view=azure-devops&tabs=yaml)
- [How to create a multi platform job on Azure Pipelines](https://10xlearner.com/2020/11/11/how-to-create-a-multi-platform-job-on-azure-pipelines/)

