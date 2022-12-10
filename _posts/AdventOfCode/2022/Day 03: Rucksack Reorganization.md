# AOC 2022 Day 3: Rucksack Reorganization

Hi there ! I'm Xavier Jouvenot and today we are continuing with the third [Advent Of Code](https://adventofcode.com) puzzle of the year 2022. 🌟

We are going to tackle the problem from the 3rd December 2022 (as you guessed), named "Rucksack Reorganization".
The solution I will propose in c++, but the reasoning can be applied to other languages.

_Self promotion_: You can find other articles on computer science and programming on my [website](www.10xlearner.com) 😉

## Part 1

### The Problem

The full version of this problem can be found directly on the [Advent of Code website](https://adventofcode.com/2022/day/3), I will only describe the essence of the problem here:

We have to help the elves with their rucksacks today, as they have trouble with them. They have misplaced some items in the rucksack and need to find them. In order to do so, they gave us the list of item in each rucksack, which look like:

```txt
vJrwpWtwJgWrhcsFMMfFFhFp
jqHRNqRjqzjGDLGLrsFMfFZSrLrFZsSL
PmmdzqPrVvPwwTWBwg
wMqvLMZHhHMvwLHjbvcjnnSBnvTQFn
ttgJtRGJQctTZtZT
CrZsJsPPZsGzwwsLwLmpwMDw
```

, with each letter (case-sensitive) being an item and each line being one rucksack. Each rucksack is actually composed of two compartments (the first half of the rucksack's item is the first compartment, and the second half is the second one), and we have to find which item is in both compartments of the rucksack.

In order to have one final answer, the elves give us a priority value for each item:
- Lowercase item types have priorities 1 through 26.
- Uppercase item types have priorities 27 through 52.

So we need to find the sum of priorities of all the items we are looking for.

### Solution

Here is the complete solution, so that you can get a feel about it, but don't worry, we are going to explain it, step by step 😉

```cpp
using aoc_2022::Day3Solver;

using Item = char;
using ItemTypePriority = size_t;
using RucksackContent = std::string_view;

ItemTypePriority letterToPriority(const Item letter) {
 if (std::islower(letter)) {
   return letter - 'a' + 1;
 }
 return letter - 'A' + 1 + 26;
}

PuzzleSolution computeSolution(const std::string_view input) {
 ItemTypePriority propertySum{0};

 for (auto it = input.begin(); it != input.end();) {
   const auto endOfLine = std::find(it, input.end(), '\n');
   const auto secondCompartiment =
       std::next(it, std::distance(it, endOfLine) / 2);

   std::vector<Item> itemAlreadyChecked;
   for (; it != secondCompartiment; it = std::next(it)) {
     if (std::ranges::any_of(itemAlreadyChecked, [&it](const auto letter) {
           return letter == *it;
         })) {
       continue;
     }
     const auto isItemInBothCompartiment =
         std::any_of(secondCompartiment, endOfLine,
                     [&it](const auto letter) { return letter == *it; });

     if (isItemInBothCompartiment) {
       propertySum += letterToPriority(*it);
       itemAlreadyChecked.emplace_back(*it);
     }
   }

   it = std::next(endOfLine);
 }

 return propertySum;
}
```

The first thing we do is to create some aliases in order to know what elements we are going to use: `Item`, `ItemPriority` and `RucksackContent`. Then, we create a simple function which is able to give us the priority value for an item:
```cpp
ItemTypePriority letterToPriority(const Item letter) {
 if (std::islower(letter)) {
   return letter - 'a' + 1;
 }
 return letter - 'A' + 1 + 26;
}
```

It works because letters are encoded as [char](https://en.cppreference.com/w/cpp/keyword/char) (a number between `-128` and `127`) and the letter are sorted alphabetically, meaning that `'b'` is equal to `'a' + 1`, `'c'` is `'b' + 1`, etc... and the same is for uppercase letters.

With this handy function, we are now ready to dive into the main algorithm solving this problem.

First of all, for each rucksack, we identify the boundaries (`it`, `secondCompartiment` and `endOfLine`) of each compartment, with [std::find](https://en.cppreference.com/w/cpp/algorithm/find), [std::next](https://en.cppreference.com/w/cpp/iterator/next) (relying heavily on iterators).

Then, we iterate through each item of the first compartment and look for the same item in the second compartment with the functions. To achieve this, we have to check the item we are looking at, has not already be checked (we don't want to count several times the same item) with the function [std::ranges::any_of](https://en.cppreference.com/w/cpp/algorithm/ranges/all_any_none_of) and a [std::vector](https://en.cppreference.com/w/cpp/container/vector) variable. If that check is negative, then, we can check is the item is also in the second compartment using [std::any_of](https://en.cppreference.com/w/cpp/algorithm/all_any_none_of), and add the priority value, if this last check is positive.

Once we have finished iterating through the item of the compartment, we go to the next rucksack, and do it all over again. And once we have go through all the rucksack, we are done and can look at our final value of `propertySum` 🙂

## Part 2

### The Problem

After successively resolving the first part, the elves have another one for use.

Each triplet of rucksacks has the same badge (item) in them, but they don't know what the badges look like.
Since it helps identify to which group the elves are, we need to find the item which is the badge for each group of three rucksacks: the badge is the only item that you can find in the three rucksacks.

Like for the previous part, we will use the sum of priorities of the items we found.

### Solution

Keeping the handy function used in the previous part, we can create a completely new algorithm for this second part:

```cpp
PuzzleSolution computeSolution(const std::string_view input) {
 ItemTypePriority propertySum{0};

 for (auto it = input.begin(); it != input.end();) {
   const auto endOfLineRucksack1 = std::find(it, input.end(), '\n');
   const auto startOfLineRucksack2 = std::next(endOfLineRucksack1);
   const auto endOfLineRucksack2 =
       std::find(startOfLineRucksack2, input.end(), '\n');
   const auto startOfLineRucksack3 = std::next(endOfLineRucksack2);
   const auto endOfLineRucksack3 =
       std::find(startOfLineRucksack3, input.end(), '\n');

   for (; it != endOfLineRucksack1; it = std::next(it)) {
     const auto isItemInRuckscak2 =
         std::any_of(startOfLineRucksack2, endOfLineRucksack2,
                     [&it](const auto letter) { return letter == *it; });
     const auto isItemInRuckscak3 =
         std::any_of(startOfLineRucksack3, endOfLineRucksack3,
                     [&it](const auto letter) { return letter == *it; });

     if (isItemInRuckscak2 && isItemInRuckscak3) {
       propertySum += letterToPriority(*it);
       break;
     }
   }

   it = std::next(endOfLineRucksack3);
 }

 return propertySum;
}
```

Like previously, we start by identifying the boundaries (`it`, `endOfLineRucksack1`, `startOfLineRucksack2`, `endOfLineRucksack2`, `startOfLineRucksack3` and `endOfLineRucksack3`) that we are going to used for the main logic of the algorithm, with [std::find](https://en.cppreference.com/w/cpp/algorithm/find), [std::next](https://en.cppreference.com/w/cpp/iterator/next).

Then, we iterate through each item of the first rucksack, and look at the other two rucksacks to see if we can find a similar item, using [std::any_of](https://en.cppreference.com/w/cpp/algorithm/all_any_none_of), and, if we can, we add the item priority value to our result variable, and we go to the next three rucksacks.

Once we have gone through each triplet of rucksacks, we are done !! And we can display the `propertySum` we found to successfully finish today's problems.

## Conclusion

Today's problem was interesting! Being organized with the boundaries variable and the standard C++ functions really speeds up the implementation of the solutions and makes it far simpler than if I had to "manually" go in each rucksack. 😉

You can note that the solutions written in this post, don't include all the sources to make running programs, but only the interesting part of the sources to solve this problem.

If you want to see the programs from end to end, you can go on my [GitHub account](https://github.com/Xav83/advent_of_code/tree/main), explore the full solution, add comments or ask questions if you want to, on the platform you read this article, it will also help me improve the quality of my articles.

---------------

Thank you all for reading this article,
And until my next article, have a terrific day 😉

## Interesting links

- [Advent of Code website](https://adventofcode.com/2022)
- C++ keywords: [char](https://en.cppreference.com/w/cpp/keyword/char), [break](https://en.cppreference.com/w/cpp/language/break), [continue](https://en.cppreference.com/w/cpp/language/continue), and [using](https://en.cppreference.com/w/cpp/language/type_alias)
- C++ functions: [std::any_of](https://en.cppreference.com/w/cpp/algorithm/all_any_none_of), [std::distance](https://en.cppreference.com/w/cpp/iterator/distance), [std::find](https://en.cppreference.com/w/cpp/algorithm/find), [std::islower](https://en.cppreference.com/w/cpp/string/byte/islower), [std::next](https://en.cppreference.com/w/cpp/iterator/next) and [std::ranges::any_of](https://en.cppreference.com/w/cpp/algorithm/ranges/all_any_none_of)
- C++ structures: [std::string_view](https://en.cppreference.com/w/cpp/string/basic_string_view) and [std::vector](https://en.cppreference.com/w/cpp/container/vector)
