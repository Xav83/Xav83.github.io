# How to get the version of a program in CMake

I'm Xavier Jouvenot and in this small post, we are going to see how to get the version of a program in CMake.

_Self promotion_: You can find other articles on my [website](www.10xlearner.com) :wink:

## Problematic

When interacting with other programs through the cmake language, you may have encountered some problem when updating those programs, such has a feature not available anymore, or a modification which broke the cmake script. In such case, being able to know which version the program installed on the machine can be very helpful ! :smile:

Moreover, if you want your cmake script to work with several versions of a program, so that everyone can use it, then you absolutely need to know the version of the program to react accordingly depending on the features available to you.

For example, I'm currently working on a cmake script to help people to use the software [pluginval](https://github.com/Tracktion/pluginval).
To make sure that the supported version of pluginval is installed, I had to found a solution to this problem.

_Note_: As I write this blog post, the cmake script for pluginval that I mentioned is far from being completed.
But, hopefully, at the time when you will be reading those lines, it will be fully working and used by other developers.
If you are interested, you will find the link to the repository of this project at the end of this article.

## Solution

Before trying to get the version of a program installed on a machine, the first thing we have to do is to check if the program is indeed installed on the machine !

#### Is the program installed

In CMake, such a function could look like that:

```cmake
set(PROGRAM_NAME my_program)
set(PROGRAM_HINT_PATHS /some/paths/to/look/in)

function(${PROGRAM_NAME}_is_installed output_var)
  unset(${PROGRAM_NAME}_EXECUTABLE CACHE)
  find_program(${PROGRAM_NAME}_EXECUTABLE
	       NAMES ${PROGRAM_NAME}
	       HINTS ${PROGRAM_HINT_PATHS}
	       DOC "${PROGRAM_NAME} executable")

  if(NOT ${PROGRAM_NAME}_EXECUTABLE)
    set(${output_var} FALSE PARENT_SCOPE)
  else()
    set(${output_var} TRUE PARENT_SCOPE)
  endif()
endfunction()
```

Don't worry, we are going to dive into the details of this function so that you can craft a similar one for your program of choice.

First, let's talks about the two first lines.
They are here to be specified by you to set your own program name and its potential location.

Then, the function that actually does the job !
In this function, we start by removing any value to the variable `${PROGRAM_NAME}_EXECUTABLE`, before filling it with the instruction `find_program`.
The call to the instruction `unset` is mandatory as the instruction `find_program` will modify the value in the variable only if there is no value already cached in it.

Finally, we check to see if the program has been found by verifying if the variable has been set, and we store the result in the output variable given by the user in the parameters of the function.

And with that, we are able to see if the program is well installed on the machine :smile:

_Note_: You can look at an example of such function by following this [link](https://github.com/Xav83/pluginval_cmake_integration/blob/ab2422980c932ab8099584b1224fd758d8b4c001/module/pluginval.cmake#L5) for the program [pluginval](https://github.com/Tracktion/pluginval)

#### Let's get the version of this program

Now that we know how to check if a program is installed, we can create a function to get the version of a program.

Here is the detail of what it looks like:

```cmake
set(PROGRAM_NAME my_program)

function(${PROGRAM_NAME}_version output_var)
  ${PROGRAM_NAME}_is_installed(IS_${PROGRAM_NAME}_INSTALLED)
  if(NOT ${IS_${PROGRAM_NAME}_INSTALLED})
    message(FATAL_ERROR "${PROGRAM_NAME} must be installed on the machine!)
  endif()

  # Runs the command to get the pluginval version
  execute_process(COMMAND ${PROGRAM_NAME} --version
                  OUTPUT_VARIABLE ${PROGRAM_NAME}_VERSION_RAW_OUTPUT)

  # Extracts the version from the output of the command run before
  string(STRIP ${${PROGRAM_NAME}_VERSION_OUTPUT} ${PROGRAM_NAME}_VERSION_OUTPUT)

  set(${output_var} ${${PROGRAM_NAME}_VERSION_OUTPUT} PARENT_SCOPE)
endfunction()
```

Like for the previous function, we start by specifying the elements that you will have to specify to adapt this function to your needs : the program name.

Then, once again, the function that does the actual job ! :wink:
First, we start by checking if the program is installed on the machine.
If not, we trigger an error, since we can't get the version of a program not installed.

Then, we run the command allowing us to get the version of the program.
All the programs a little bit used have such a command.
By conversion, using the flag `--version` or `-v` allows to display the version of the program.
If this is not the case, such a flag should be specified in the documentation.

Now that we have the output given by the command, all we have to do, is to extract the version from the string.
To do so, in this example, we have only removed the leading and trailing spaces, but depending on the output given by the command, you may have to remove some others characters.
By using some [cmake string instructions](https://cmake.org/cmake/help/v3.12/command/string.html), you will end up with only the version of the program in the variable `${PROGRAM_NAME}_VERSION_OUTPUT`.

And finally, we set the output variable given by the user with the version of the program. :smile:

With such function, getting the version of your program becomes very easy:

```cmake
set(PROGRAM_NAME my_program)

${PROGRAM_NAME}_version(${PROGRAM_NAME}_VERSION)

message(STATUS "${PROGRAM_NAME} version detected : ${${PROGRAM_NAME}_VERSION}")
```

_Note_: You can look at an example of such function by following this [link](https://github.com/Xav83/pluginval_cmake_integration/blob/ab2422980c932ab8099584b1224fd758d8b4c001/module/pluginval.cmake#L24) for the program [pluginval](https://github.com/Tracktion/pluginval)

#### What's next ?

In CMake, there is a function which does something similar to what we want, allynd this function is named [cmake_minimum_required](https://cmake.org/cmake/help/latest/command/cmake_minimum_required.html).
This function sets the minimum required version of cmake for a project.

Such feature goes beyond the scope of this article, but can be a nice to have if you plan to make support for some program easier in your CMake scripts.
Implementing such functions can also be a nice exercise in CMake :wink:

--------

Thank you all for reading this article,
And until my next article, have a splendid day :wink:

## Interesting links

- [Pluginval GitHub repository](https://github.com/Tracktion/pluginval)
- [Pluginval Cmake Integration GitHub repository](https://github.com/Xav83/pluginval_cmake_integration)
- [cmake_minimum_required cmake documentation](https://cmake.org/cmake/help/latest/command/cmake_minimum_required.html)
- [find_program cmake documentation](https://cmake.org/cmake/help/latest/command/find_program.html)
- [string cmake documentation](https://cmake.org/cmake/help/latest/command/string.html)
- [unset cmake documentation](https://cmake.org/cmake/help/latest/command/unset.html)
- [execute_process cmake documentation](https://cmake.org/cmake/help/latest/command/execute_process.html)
- [set cmake documentation](https://cmake.org/cmake/help/latest/command/set.html)
