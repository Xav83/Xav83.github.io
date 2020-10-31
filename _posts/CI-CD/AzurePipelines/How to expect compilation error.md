# How to expect a compilation error with Azure Pipelines

Hello ! I'm Xavier Jouvenot and in this small post, we are going to see how to expect a compilation error with Azure Pipelines.

_Self promotion_: You can find other articles on my [website](www.10xlearner.com)

## Problematic

Most of the time, when developers interacts with Azure Pipelines, and CI in general, they want to make sure that their program compiles and their tests pass.
But, depending on the program you create, or the system you are putting in place, you may want to check if an error is generated when one error must be generated.

For example, I encountered this problematic when playing with the tool [cppcheck](http://cppcheck.sourceforge.net/), and I wanted to make sure that I was able to make it trigger an error in a small program.
To do such test, several solutions are available to us : we can, for example, create a script or a program which is going to test this behavior and expect the error. In that case, this script will be like any other tests and you will face no particular problem. It will only take some time to create such script.

The other solution, and the one this article is about, is to be able to expect the error directly in the CI, in that case, Azure Pipelines. :wink:

## Solution

Here is an example to how to expect an error in an Azure Pipeline step:

```yml
steps:
    - bash: |
        cd build
        cmake --build .
        error_code=$?
        if [[ $error_code -eq 0 ]]; then
            exit 1
        fi
        exit 0
```

Let's dive a little bit in what is happening here ! :smile:

In this extract of a `azure-pipeline.yml` file, we create a `bash` step.
In this step, we are compiling a project using CMake. You can note that the technology is not important, I took this example from a project where I used CMake, but you can replace it by any command supposed to succeed.

Once the operation is over, we get the "result" of the operation with the command `error_code=$?`, in a variable named `error_code`.
In bash, and shell, when a command succeed, it is supposed to return an error code equals to 0; whereas when it fails, it should return any value except 0.

Finally, we look at the value of the variable `error_code` to check if there has been an error during the compilation.
If the variable is evaluated to `0`, then, the compilation succeeded and and we exit the step by returning the value `1` to make the step fail.
On the other hand, if the variable is evaluated to `1`, then, the compilation fails, which is what we are expecting, so we exit the step by returning the value `0` to make the step ends successfully.

And just like that, with a few lines, we are able to expect an error in an Azure Pipelines step. :smile:

_Note_: I have to admit that I would have loved to have a field `is_error_expected` available. It would have made this solution more elegant and easier to replicate across different project.
Hopefully, one day, such a feature will be available to us :wink:

-----------

Thank you all for reading this article,
And until my next article, have a splendid day.

## Interesting links

-  [Azure Pipelines documentation](https://docs.microsoft.com/en-us/azure/devops/pipelines/yaml-schema?view=azure-devops&tabs=schema%2Cparameter-schema)
