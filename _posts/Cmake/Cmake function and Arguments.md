# How to correctly create a CMake function with proper arguments

I'm Xavier Jouvenot and in this small post, we are going to see how to correctly create a CMake function with proper arguments.

_Self promotion_: You can find other articles on my [website](www.10xlearner.com) :wink:

## Problematic

In CMake, like in many other languages, it is possible to create and use [functions](https://cmake.org/cmake/help/latest/command/function.html).

Unlike in other languages, the number of arguments when calling a CMake function is actually not checked by the system.
Indeed, you can call a function, declared as taking only one arguments, with three arguments without having CMake preventing you.

Even if this behavior is wanted by CMake developers and allows some nice features, if you don't know about it, it can be the origin of a lot of bugs and unwanted behaviors in your CMake scripts.

## Solutions

To make sure that your function has the right number of arguments when called, we have two solutions (two that I am aware of :laugh:).

#### Arguments count

This first solution consists of using a CMake variable available to us giving us the number of arguments used when the function is called.

```cmake
function(my_function)
  message("Number of parameters (including the function name)= \"${ARGC}\"")
  message("List of parameters of the function = \"${ARGN}\"")
  message("List of parameters of the function with the function name = \"${ARGV}\"")

  if(NOT ${ARGC} EQUAL 3)
    message(FATAL_ERROR "Call to 'my_function' with ${ARGC} arguments instead of 3")
  endif()
endfunction()

my_function(param1, param2)
```

With the previous sample of code, we will be able to display some very interesting informations about the numbers of arguments.
Indeed, with the variable `ARGC`, we are able to see the number of arguments and we can control it by using some conditions, for example.

I have also added some other variables, `ARGN` and `ARGV`, so that you can have some knowledge about them, if you want to be able to manipulate or to extract more informations about the parameters passed to your own Cmake functions :wink:.

Even if this function is simple to explain, it requires you to write a lot of code to each check each parameter, and to make sure that they are all what you function expects.
Moreover, since the CMake functions tends to have a lot of parameters, it can become a lot of pain to check all of them that way.

#### cmake_parse_arguments function

To face function with a big number of parameters and with different "kinds" of parameters, the CMake developers have created a new feature very interesting.
Indeed, they have given us the ability to add keywords in our functions parameters so that we could group them by type, and check their validity more easily.
Moreover, it even allow us to handle an infinity of parameters even more easily :wink:

```cmake
function(my_function)
  set(options IS_ACTIVATED)
  set(oneValueArgs NAME)
  set(multiValueArgs ARRAY_OF_PARAM)
  cmake_parse_arguments(MY_FUNCTION_REQUIRED "${options}"
                        "${oneValueArgs}" "${multiValueArgs}" ${ARGN})

  # If the name is not valid
  if(MY_FUNCTION_NAME STREQUAL "Bad name")
    message(FATAL_ERROR "Error : The name passed as an argument is not valid.")
  endif()

  # If zero version are passed
  if(NOT DEFINED MY_FUNCTION_ARRAY_OF_PARAM)
    message(FATAL_ERROR "Missing ARRAY_OF_PARAM, please add one")
  endif()

  list(LENGTH MY_FUNCTION_ARRAY_OF_PARAM NUMBER_OF_PARAM_IN_ARRAY)

  # If more than three versions are passed
  if(${NUMBER_OF_PARAM_IN_ARRAY} GREATER_EQUAL 3)
    message(
      FATAL_ERROR
        "Too much param (${NUMBER_OF_PARAM_IN_ARRAY}) given. Max : 3"
    )
  endif()

  # Rest of the function
  # ...
endfunction()
```

Quite some code, isn't it :laugh:

Let's dive in !
In this sample of code, we create a function which can be called like that : `my_function(NAME "John Doe" ARRAY_OF_PARAM "First param" "Second Param" IS_ACTIVATED).`

The keyword `IS_ACTIVATED` is actually a flag that can be omit and can be used to trigger some code in the function or modify its behavior, for example.
This means that, if it is passed to the function, then, the variable `MY_FUNCTION_IS_ACTIVATED` will be set to `TRUE`, else it will be set to `FALSE`.

The keyword `NAME` is used to pass only one argument to the function.
Since we don't check if it is defined in the function, this means that it is not mandatory, but the string "Bad name" is consider a wrong argument when associated with this keyword.

Finally, the keyword `ARRAY_OF_PARAM` is used to pass several arguments to the function.
Since we check if that this keyword is defined and the number of parameters given to it, it makes it mandatory for people calling this function to pass this keyword to the function, and to make it have less than 3 arguments associated.

Using the function [cmake_parse_arguments](https://cmake.org/cmake/help/latest/command/cmake_parse_arguments.html) makes the management of parameters for Cmake function easier.
It allows us to have functions with a similar structure as the functions given by CMake standard, so that we will have a consistent way to use all functions across our projects.

I find this solution the best one to this day to be used in CMake.
I hope that I could convince you to give it a try, and that it will suit you need :slight_smile:

---------

Thank you all for reading this article,
And until my next article, have a splendid day :wink:

## Interesting links

- [CMake function documentation](https://cmake.org/cmake/help/latest/command/function.html)
- [cmake_parse_arguments documentation](https://cmake.org/cmake/help/latest/command/cmake_parse_arguments.html)
- [CMake arguments documentation](https://cmake.org/cmake/help/latest/command/macro.html#arguments)
- [Cmake function arguments example](https://gist.github.com/antiagainst/4614ca9ed29f5b8c0ba4)

