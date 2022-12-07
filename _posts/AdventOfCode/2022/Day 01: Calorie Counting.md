# AOC 2022 Day 1: Calorie Counting

Hi there ! I'm Xavier Jouvenot and today we are going to take a look at the first [Advent Of Code](https://adventofcode.com) puzzle of the year 🎄 (if you are from the future, you can not that the current year is 2022 😉).

For this first "Advent Of Code" post of the year, we are going to tackle the problem from the 1st December 2022 (pretty obviously), named "Calorie Counting".
The solution I will propose in c++, but the reasoning can be applied to other languages.

_Self promotion_: You can find other articles on computer science and programming on my [website](www.10xlearner.com) 😉

## Part 1

### The Problem

The full version of this problem can be found directly on the [Advent of Code website](https://adventofcode.com/2022/day/1), I will only describe the essence of the problem here:

Today, we are helping the elves, going through a forest, to manage their food. We need to know which elf has the most food, and to do so, we are going to rely on the calories. We know, for each elf, the number of items they have, and the calories of each item. Those information are given like so:

```txt
1000
2000
3000

4000

5000
6000
```

For example, here, we have the list of items for 3 elves (separated by the empty lines).

Our first mission of the year is to find the maximum amount of calories detained by one elf.

### Solution

Here is the complete solution, so that you can get a feel about it, but don't worry, we are going to explain it, step by step 😉

```cpp
using Calories = size_t;

PuzzleSolution computeSolution(const std::string_view input) {
 auto it = input.begin();
 Calories maxCaloriesCarryByOneElf = 0;
 Calories caloriesCarriedByCurrentElf = 0;

 while (it != input.end()) {
   if (*it == '\n') {
     maxCaloriesCarryByOneElf =
         std::max(maxCaloriesCarryByOneElf, caloriesCarriedByCurrentElf);
     caloriesCarriedByCurrentElf = 0;
     ++it;
     continue;
   }
   auto endOfNumber = std::find(it, input.end(), '\n');
   caloriesCarriedByCurrentElf += std::stoi(std::string(it, endOfNumber));
   it = std::next(endOfNumber);
 }
 // last elf
 maxCaloriesCarryByOneElf =
     std::max(maxCaloriesCarryByOneElf, caloriesCarriedByCurrentElf);

 return maxCaloriesCarryByOneElf;
}
```

First of all, we start by associating explicitly a type for the `Calories`, with the instruction `using Calories = size_t;`. I took `size_t` to make sure that we won't have any overflow, but the main advantage is that it makes the code easier to understand.

Now, let's explain how this algorithm works: we are going to iterate through each line of our input, using iterators, and react accordingly: if this is an empty line (`*it == '\n'`), then we check if the calories accumulated for the current elf in the input is the biggest amount we have found so far (with [std::max](https://en.cppreference.com/w/cpp/algorithm/max)), and update our variable `maxCaloriesCarryByOneElf` accordingly, before [continue](https://en.cppreference.com/w/cpp/language/continue) to the next line; if the line is **not** empty, then we extract the number of calories using [std::find](https://en.cppreference.com/w/cpp/algorithm/find) to know when the number ends and [std::stoi](https://en.cppreference.com/w/cpp/string/basic_string/stol) to convert the `string` into a number, before adding it to our variable `caloriesCarriedByCurrentElf` and we continue to the next line.

Finally, once we have gone through all the input, we check if the latest element is the max (since the loop ends before checking the calories of the last elf) before returning the biggest value found, the result that we were looking for. 😊

## Part 2

### The Problem

The situation is the same as for the first part, but, this time, we want to know the amount of calories detained by the Top 3 of elves, to be able to gather more food easily.

### Solution

By adapting the solution to the previous part, we can get the solution to this part !

```cpp
PuzzleSolution computeSolution2(const std::string_view input) {
 auto it = input.begin();
 std::vector<Calories> maxCaloriesCarryByTopThreeElves = {0, 0, 0};
 Calories caloriesCarriedByCurrentElf = 0;

 while (it != input.end()) {
   if (*it == '\n') {
     maxCaloriesCarryByTopThreeElves.emplace_back(caloriesCarriedByCurrentElf);
     maxCaloriesCarryByTopThreeElves.erase(
         std::min_element(maxCaloriesCarryByTopThreeElves.begin(),
                          maxCaloriesCarryByTopThreeElves.end()));
     caloriesCarriedByCurrentElf = 0;
     ++it;
     continue;
   }
   auto endOfNumber = std::find(it, input.end(), '\n');
   caloriesCarriedByCurrentElf += std::stoi(std::string(it, endOfNumber));
   it = std::next(endOfNumber);
 }

 // last elf
 maxCaloriesCarryByTopThreeElves.emplace_back(caloriesCarriedByCurrentElf);
 maxCaloriesCarryByTopThreeElves.erase(
     std::min_element(maxCaloriesCarryByTopThreeElves.begin(),
                      maxCaloriesCarryByTopThreeElves.end()));

 return std::accumulate(maxCaloriesCarryByTopThreeElves.begin(),
                        maxCaloriesCarryByTopThreeElves.end(), 0);
}
```

Let's see what the main differences with the previous part are !

Here, instead of using a `Calories` variable to store the biggest amount of calories found on one elf, we use a `std::vector<Calories>` to store the Top 3 biggest.

This means that, when we encounter an empty line, we have to see if the amount of calories found for the elf can make it in our Top 3, so far. To achieve that, we add the number found in our vector (with [std::vector::emplace_back](https://en.cppreference.com/w/cpp/container/vector/emplace_back)), before removing the smallest element from our vector to keep the 3 biggest one (using [std::vector::erase](https://en.cppreference.com/w/cpp/container/vector/erase) and [std::min_element](https://en.cppreference.com/w/cpp/algorithm/min_element)).

Finally, we compute our solution by adding the 3 biggest amount of calories we found using [std::accumulate](https://en.cppreference.com/w/cpp/algorithm/accumulate) 🎉

## Conclusion

I have been solving Advent Of Code problems from time to time now, and it is nice to start this year's problem easily (even if I am a little late 😄). The next problem is probably going to be harder, but it is good practice so I definitely encourage you to try it yourself, and send me your solution (even if it is in another language). And if you have some ideas to improve my solution, feel free to comment on it too ! I will be happy to read them all 😃

You can note that the solutions written in this post, don't include all the sources to make running programs, but only the interesting part of the sources to solve this problem.

If you want to see the programs from end to end, you can go on my [GitHub account](https://github.com/Xav83/advent_of_code/tree/main), explore the full solution, add comments or ask questions if you want to, on the platform you read this article, it will also help me improve the quality of my articles.

---------------

Thank you all for reading this article,
And until my next article, have a splendid day 😉

## Interesting links

- [Advent of Code website](https://adventofcode.com/2022)
- C++ keywords: [using](https://en.cppreference.com/w/cpp/keyword/using) and [continue](https://en.cppreference.com/w/cpp/language/continue)
- C++ functions: [std::max](https://en.cppreference.com/w/cpp/algorithm/max), [std::find](https://en.cppreference.com/w/cpp/algorithm/find), [std::stoi](https://en.cppreference.com/w/cpp/string/basic_string/stol), [std::vector::emplace_back](https://en.cppreference.com/w/cpp/container/vector/emplace_back), [std::min_element](https://en.cppreference.com/w/cpp/algorithm/min_element) and [std::accumulate](https://en.cppreference.com/w/cpp/algorithm/accumulate)
- C++ structure: [std::vector](https://en.cppreference.com/w/cpp/container/vector)
