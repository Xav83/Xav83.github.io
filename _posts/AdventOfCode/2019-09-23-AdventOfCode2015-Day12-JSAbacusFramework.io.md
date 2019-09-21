# Advent Of Code - JSAbacusFramework.io - Puzzle 12

Hello ! I'm Xavier Jouvenot and here is the twelve part of a long series on [Advent Of Code](https://adventofcode.com). You can find the previous part [here](2019-09-16-AdventOfCode2015-Day11-Corporate-Policy.md)

For this new post, we are going to solve the problem from the 12th December 2015, named "JSAbacusFramework.io".
The solution I will propose in C++, but the reasoning can be applied to other languages.

## Part 1

### The Problem

The full version of this problem can be found directly on the [Advent of Code website](https://adventofcode.com/2015/day/12), I will only describe the essence of the problem here:

Santa's Accounting-Elves need help balancing the books after a recent order.
All the information you have is a JSON document with variety of things (array, objects, number and strings).

What we have to do then, is to find all the numbers and to sum them.

### Solution

Well first of all, we have to be able to parse the information from a JSON in C++.
We could write a JSON parser from scratch to do it, but that would be a long and hard work, and I don't know for you, but I don't want to do work that is already done.
So I've decided to work with the [JSON for Modern C++](https://github.com/nlohmann/json) library of nlohmann which allow us to have a easily readable code to handle JSON documents.

To include this library, I've used a combination of [Conan](https://conan.io/) and [Cmake](https://cmake.org/), but I won't go into detail here.
You can look at it on the [Github repository](https://github.com/Xav83/AdventOfCode/tree/2015.12/2015/Day12).
Now that everything is clear about the library, let's get the problem solved.

First of all, we have to get the JSON document ready to be used:

```c++
#include <nlohmann/json.hpp>

std::ifstream file (pathToJsonDocument);
nlohmann::json jsonDocument;
file >> jsonDocument;
```

Simple, right ?! We get the file and then only give it to the JSON library to be ready to be used.

Now, how are we going to get the sum of all the numbers in the JSON document.
As explained in the problem, there are 4 types of elements in the JSON document : array, objects, number and strings.

So we have to go through the JSON elements and define what to do with them.
We can ignore the string, since there are no numbers in them.
If we find a number, all we have to do is to add it to the sum.
But the more difficult part is about the arrays and the objects.
Indeed, they can contains the for types of objects too, and that mean only one thing for our solution (suspense ...)🥁 : recursion.

Now that we have describe what we have to do, let's look a look at the actual method doing this:

```c++
int getSum (const nlohmann::json& jsonDocument)
{
    auto sum{0};
    for (const auto& [key, value] : jsonDocument.items())
    {
        if(value.is_number())
        {
            sum += value.get<int>();
        }
        else if(value.is_array() || value.is_object())
        {
            sum += getSum (value);
        }
    }
    return sum;
}
```

I love how the JSON library make this function so much readable.
As you can see, we have a recursion call for the arrays and the object, and each time we found a number, we add it to the global sum. And this is it. All we have to do is to call this method with the JSON we have created and we are good. We have the solution of our problem 😃

## Part 2

### Problem

The Accounting-Elves have realized that they double-counted everything red.
So we have now to ignore any object (and all its children) which has any property with the value "red".
Only the objects, not the array.

### Solution

So now, we no longer ignore the strings.
By adding the following condition in the previous method, we will complete the demand of the Accounting-Elves.

```c++
for (const auto& [key, value] : jsonDocument.items())
{
    // other conditions
    else if(value.is_string() && jsonDocument.is_object())
    {
        if(value.get<std::string>() == "red")
        {
            sum = 0;
            break;
        }
    }
}
```

Let's explain what this condition actually does.
We start by looking if the element is a string and we are in an object, so that we meet the first part of the criteria.
If this is the case, we look at the value of the string, then we meet the last requirement, so we can stop including element to the sum of number.
So we start by resetting the currently processing sum to 0. This sum is the one for the object, so resetting it, will also avoid to have the previously parsed elements added in the global sum, and then, we break from the for loop to not process the other elements of the object.

And voilà. we have a function allowing us to find the solution of this second part. 😃

## Conclusion

You can note that the solutions, written in this post, don't include all the sources to make running programs, but only the interesting part of the sources to solve this problem.
If you want to see the programs from end to end, you can go on my [GitHub account](https://github.com/Xav83/AdventOfCode/tree/2015.12/2015/Day12), explore the full solution, add comments or ask questions if you want to, on the platform you read this article, it will also help me improve the quality of my articles.

Here is the list of elements that we have used, I can't encourage you enough to look at their definitions :

- [std::ifstream](https://en.cppreference.com/w/cpp/io/basic_ifstream)
- [std::string](https://en.cppreference.com/w/cpp/string/basic_string)
- [nlohmann::json](https://github.com/nlohmann/json)

Thanks for you reading, hope you liked it 😃

And until next part, have fun learning and growing.
