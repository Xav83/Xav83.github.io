# Advent Of Code - No Such Thing as Too Much - Puzzle 17

Hello ! I'm Xavier Jouvenot and here is the part seventeenth of a long series on [Advent Of Code](https://adventofcode.com). You can find the previous part [here](2019-11-11-AdventOfCode2015-Day16-Aunt-Sue.md)

For this new post, we are going to solve the problem from the 17th December 2015, named "No Such Thing as Too Much".
The solution I will propose in C++, but the reasoning can be applied to other languages.

## Part 1

### The Problem

The full version of this problem can be found directly on the [Advent of Code website](https://adventofcode.com/2015/day/17), I will only describe the essence of the problem here:

The elves bought too much eggnog (150 liters), so we have to make fit in the containers in the fridge.
All we want to do is to found out how many different combinations of containers can exactly fit all 150 liters of eggnog, when filling all containers entirely ? 🤔

### Solution

Several approaches can be used to discover the solution of this problem.

My first idea was to create calculate the volume that could be store in all the combinations of containers.
It worked well for 5 containers, but when I gave it the real input, with 20 containers, the execution time exploded. I actually never went through since there are so much combinations to check.

After this failure, I've tried a second (and better 😉) approach.
This solution consists of putting the eggnog in one container at the time and check if we still have eggnog left. If so, put some in another one, and check again, and if at any point, their not enough eggnog to fill the container, or if we successfully get stored perfectly the eggnog, then, we go find the next combination of containers, which means recursivity (yeah! \o/).

But enough explanation, let's dive in the code.
Actually, there will be one main function with the algorithm in it:

```c++
using Container = size_t;
using Containers = std::vector<Container>;

size_t countNumberOfCombination(const Containers &containers, size_t volum) {
  size_t numberOfCombination{0};
  for (auto index = size_t{0}; index < containers.size(); ++index) {
    const auto remainingVolum = volum - containers[index];
    if (remainingVolum == 0) {
      ++numberOfCombination;
      continue;
    }
    if (remainingVolum < 0) {
      continue;
    }
    Containers containersNotUsed;
    std::copy(containers.begin() + index + 1, containers.end(),
              std::back_inserter(containersNotUsed));

    numberOfCombination +=
        countNumberOfCombination(containersNotUsed, remainingVolum);
  }
  return numberOfCombination;
}
```

Don't worry, as usual, I'm going to explain how this code works.

As parameters, we have the containers available and the volume of eggnog we have to store.
For each containers, we check if filling it with eggnog bring the volume of eggnog to 0, and if this is the case, we did it ! We have found one combination !
But if we can fill the container completely with the eggnog, then, we ignore this container, and go for the next one.
Finally, if we can fill the container and we still have some eggnog left, then, the recursion arrive, and we do those checks again, but without the container and without the volume of eggnog of the container. And once the recursive call is finished, we will have the number of combination working with the current container full, so we can add this number to the current known number of combination of containers.

All we have to do is to give to this function the data of the problem, and voilà, we know how many combination of containers there are 😄

I encourage you to look at the complete solution on my [GitHub](https://github.com/Xav83/AdventOfCode/blob/2015.17/2015/Day17/src/part_one.cpp) 😉

## Part 2

### Problem

In this part, we need to find how many combination of containers can actually contains the 150 liters of eggnog, with the minimum amount of containers.

### Solution

To solve this problem, I've decided to modify the function used for the first part, so that it looks like that:

```c++
size_t countNumberOfCombination(const Containers &containers, size_t volum,
                                size_t numberOfContainerUsed,
                                size_t &numberMinOfContainerUsed) {
  size_t numberOfCombination{0};
  for (auto index = size_t{0}; index < containers.size(); ++index) {
    const int remainingVolum = volum - containers[index];
    if (remainingVolum == 0) {
      if (numberMinOfContainerUsed > (numberOfContainerUsed + 1)) {
        numberOfCombination = 1;
        numberMinOfContainerUsed = numberOfContainerUsed + 1;
      } else {
        assert(numberMinOfContainerUsed == (numberOfContainerUsed + 1));
        ++numberOfCombination;
      }
      continue;
    }
    if (remainingVolum < 0) {
      continue;
    }
    if ((numberOfContainerUsed + 1) >= numberMinOfContainerUsed) {
      continue;
    }
    Containers containersNotUsed;
    std::copy(containers.begin() + index + 1, containers.end(),
              std::back_inserter(containersNotUsed));

    const auto currentNumberMinOfContainerUsed = numberMinOfContainerUsed;

    const auto numberOfCombinationFound = countNumberOfCombination(
        containersNotUsed, remainingVolum, numberOfContainerUsed + 1,
        numberMinOfContainerUsed);
    if (currentNumberMinOfContainerUsed == numberMinOfContainerUsed) {
      numberOfCombination += numberOfCombinationFound;
    } else {
      numberOfCombination = numberOfCombinationFound;
    }
  }
  return numberOfCombination;
}
```

This is a lot longer than in the first part. Let's explain the changes.

First of all, there are two new parameters, to keep track of the number of containers used during the recursion.

Then, there is the validation of a combination which is modified:
```c++
if (remainingVolum == 0) {
    if (numberMinOfContainerUsed > (numberOfContainerUsed + 1)) {
        numberOfCombination = 1;
        numberMinOfContainerUsed = numberOfContainerUsed + 1;
    } else {
        assert(numberMinOfContainerUsed == (numberOfContainerUsed + 1));
        ++numberOfCombination;
    }
    continue;
}
```
, in which we check if the combination found has less containers than the ones found until then.
If so, we reset the number of combination found and update the minimum of containers. If not, then, we only have to increment the number of combination found.

Before the recursion, we check if adding this container will make us exceed the minimum already found. If so, we skip directly to the next container.

And finally, the recursion:
```c++
const auto currentNumberMinOfContainerUsed = numberMinOfContainerUsed;
const auto numberOfCombinationFound = countNumberOfCombination(
    containersNotUsed, remainingVolum, numberOfContainerUsed + 1,
    numberMinOfContainerUsed);
if (currentNumberMinOfContainerUsed == numberMinOfContainerUsed) {
    numberOfCombination += numberOfCombinationFound;
} else {
    numberOfCombination = numberOfCombinationFound;
}
```

Here, we check if the minimum amount of containers has changed. If this is the case, we change the number of combinations currently known by the number found during the recursion, if not, we add the number of combination found to the one currently known.

And like that, we have our new algorithm ready to solve our problem. 😄

## Conclusion

You can note that the solutions, written in this post, don't include all the sources to make running programs, but only the interesting part of the sources to solve this problem.
If you want to see the programs from end to end, you can go on my [GitHub account](https://github.com/Xav83/AdventOfCode/tree/2015.17/2015/Day17), explore the full solution, add comments or ask questions if you want to, on the platform you read this article, it will also help me improve the quality of my articles.

Here is the list of elements that we have used, I can't encourage you enough to look at their definitions :

- [std::vector](https://en.cppreference.com/w/cpp/container/vector)
- [std::copy](https://en.cppreference.com/w/cpp/algorithm/copy)
- [std::back_inserter](https://en.cppreference.com/w/cpp/iterator/back_inserter)

Thanks for you reading, hope you liked it 😃

And until next part, have fun learning and growing.
