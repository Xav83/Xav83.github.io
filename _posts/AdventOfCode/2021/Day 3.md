# Advent Of Code 2021 - Binary Diagnostic - Puzzle 3

Hello ! I'm Xavier Jouvenot and here is the 3rd part of a long series on [Advent Of Code 2021](https://adventofcode.com).

For this new post, we are going to solve the problem from the 3rd December 2021, named "Binary Diagnostic".
The solution I will propose in C++, but the reasoning can be applied to other languages.

_Self promotion_: You can find other articles on computer science and programming on my [website](www.10xlearner.com) 😉

## Part 1

### The Problem

The full version of this problem can be found directly on the [Advent of Code website](https://adventofcode.com/2021/day/3), I will only describe the essence of the problem here:

After diving into the abyss, during the Day 2, we ear some cracking noise, so we ask the computer to give us a diagnostic report. In this report, we want to know about the **power consumption** of the submarine, but the information is encoded in a binary message which look like that:

```
00100
11110
10110
10111
10101
01111
00111
11100
10000
11001
00010
01010
```

In order to find the information we want, we need to calculate 2 rates (the `gamma rate` and the `epsilon rate`) before multiplying them to have the answer that we need. We can get each `rate` by constructing literally bit by bit. Indeed we can deduce each bit constituting the `rate` we want by using the binary diagnostic, as the first bit of each rate can be found by using the first number of each line of the binary message, second bit using the second number of each line of the binary message, etc... . For the `gamma rate`, each bit is equal to the number the **most represented**  at that position in each line, in the binary message; while, for the `epsilon rate`, each bit is equal to the number the **less represented**  at that position in each line, in the binary message.

You can look at the example given on [Advent of Code website](https://adventofcode.com/2021/day/3), if you want to look at how those rates are generated from the example.

### Solution

So, we are going to need to be able to do 2 things:
- Count the number of `0` and `1` in the binary message
- Calculate the rates accordingly

In C++, it looks like that:

```cpp
#include <algorithm>
#include <array>
#include <cassert>
#include <cmath>
#include <iostream>
#include <string>
#include <string_view>

constexpr std::array<std::string_view, 1000> input{ /* ... */ };

struct Counter
{
    size_t numberOfZero{0};
    size_t numberOfOne{0};
};

int main()
{
    auto gammaRate{0}, epsilonRate{0};
    std::array<Counter, input.front().size()> counters;

    for(const auto& binary: input)
    {
        for(auto index = 0; index < binary.size(); ++index)
        {
            const auto figure = binary[index];
            if(figure == '0') { ++(counters[index].numberOfZero) ; continue; }
            if(figure == '1') { ++(counters[index].numberOfOne) ; continue; }
            assert(false); // Wrong input
        }
    }

    for(auto index = 0; index < counters.size(); ++index)
    {
        const auto& counter = counters[index];
        const auto powerOfTwo = counters.size() - 1 - index;
        if(counter.numberOfZero > counter.numberOfOne) { gammaRate += std::pow(2, powerOfTwo); continue; }
        if(counter.numberOfZero < counter.numberOfOne) { epsilonRate += std::pow(2, powerOfTwo); continue; }
        assert(false); // equality ??!!
    }

    std::cout << "The solution is: " << (gammaRate * epsilonRate) << std::endl;

    return 0;
}
```

Let's dive a little bit in the detail of this source code ! 🙂

In the first `for` loop, we count the numbers of 0s and 1s on each line of the binary message, using an array of `Counters` with each `Counter` counting for one specific letter position in the binary messages (the position of the `Counter` in its array being the same of the position of the letters in the line).

The, in the second `for` loop, we look at the information we have gathered in the previous `for` loop, and depending on the result we got. If we look which is the most represented number, and update the corresponding `rate` accordingly to the position of the number in the binary message's line.

Finally, we display the result wanted by multiplying the 2 `rates` we have extracted from the binary message. 🙂

<details>
  <summary>[Spoiler] Problem Answer</summary>

The puzzle answer was 4006064.
</details>

## Part 2

### Problem

Now that we have found the **power consumption** of the submarine, we want to verify the **life support rating** which can be calculated by multiplying the **oxygen generator rate** by the **CO2 scrubber rate**. Fortunately, we can found those last two in the same diagnostic report, by filtering values in the binary message over and over until you get the `rates` we want.

That filtering process goes as follow:
- Consider just the first bit of each line
- Find the most common (for the **oxygen generator rate**), or the least common (for the **CO2 scrubber rate**), bit at this position of the line.
- Discard all the lines that don't match the bit you where looking for
- If you have only one line left, it is the rate you are looking for, so you have to convert from binary to decimal to know the value of the rate.
- If you still have several lines left, then, repeat the same process with the lines you have left.


### Solution

For this part, the solution is going to look a little more complex.
Indeed, we have a lot of steps to go through and cases to handle ! The solution I have found can probably be improved, so you can always comment about an amelioration you think that could be done 😉
Moreover, I only going to show the code associated with the process of filtering the lines of the binary message, and calculating the solution.
If you want to look at the full code, you can do in the [file versioned on GitHub](https://github.com/Xav83/adventofcode2021/blob/main/Day%203/src/part2.cpp) 😉

First of all, we copy the binary message to filter out each rate at the same time:
```cpp
constexpr std::array<std::string_view, 1000> input{ /* ... */ };

/* ... */

std::vector<std::string_view> potentialsOxygenGeneratorRating(input.begin(), input.end());
std::vector<std::string_view> potentialsCo2ScrubberRating(input.begin(), input.end());
```

Then, we iterate in the binary message letter by letter:
```cpp
for(auto index = 0; index < input.front().size(); ++index)
```

Since `input.front()` is one line of the binary message, then it's size is the number of letter in it, and the variable `index` is the position of the letter in that line.

So, we start by counting the number of `0`s and `1`s at that position in each line, of the potential rates (02 and C02):
```cpp
struct Counter
{
    size_t numberOfZero{0};
    size_t numberOfOne{0};
};

/* ... */

Counter co2Counter, o2Counter;
for(const auto& binary: potentialsCo2ScrubberRating)
{
    const auto figure = binary[index];
    if(figure == '0') { ++(co2Counter.numberOfZero) ; continue; }
    if(figure == '1') { ++(co2Counter.numberOfOne) ; continue; }
    assert(false); // Wrong input
}

for(const auto& binary: potentialsOxygenGeneratorRating)
{
    const auto figure = binary[index];
    if(figure == '0') { ++(o2Counter.numberOfZero) ; continue; }
    if(figure == '1') { ++(o2Counter.numberOfOne) ; continue; }
    assert(false); // Wrong input
}
```
In this code, we actually do the same operation (of counting the `0`s and `1`s) on 2 different lists (`potentialsCo2ScrubberRating` and `potentialsOxygenGeneratorRating`).

And now, we filter the lines which doesn't match the criteria of each rate:
```cpp
if(potentialsCo2ScrubberRating.size() != 1) {
  if(co2Counter.numberOfZero > co2Counter.numberOfOne) {
      /* removes the binaries with a 0 in this position for CO2 */
      potentialsCo2ScrubberRating.erase(std::remove_if(potentialsCo2ScrubberRating.begin(), potentialsCo2ScrubberRating.end(),
                                                          [index](const auto& binary){ return binary[index] == '0'; }),
                                          potentialsCo2ScrubberRating.end());
  }
  else {
      /* removes the binaries with a 1 in this position for CO2 */
      potentialsCo2ScrubberRating.erase(std::remove_if(potentialsCo2ScrubberRating.begin(), potentialsCo2ScrubberRating.end(),
                                                          [index](const auto& binary){ return binary[index] == '1'; }),
                                          potentialsCo2ScrubberRating.end());
  }
}

if(potentialsOxygenGeneratorRating.size() != 1) {
  if(o2Counter.numberOfZero > o2Counter.numberOfOne) {
      /* removes the binaries with a 1 in this position for O2 */
      potentialsOxygenGeneratorRating.erase(std::remove_if(potentialsOxygenGeneratorRating.begin(), potentialsOxygenGeneratorRating.end(),
                                                          [index](const auto& binary){ return binary[index] == '1'; }),
                                          potentialsOxygenGeneratorRating.end());
  }
  else {
      /* removes the binaries with a 0 in this position for O2 */
      potentialsOxygenGeneratorRating.erase(std::remove_if(potentialsOxygenGeneratorRating.begin(), potentialsOxygenGeneratorRating.end(),
                                                          [index](const auto& binary){ return binary[index] == '0'; }),
                                          potentialsOxygenGeneratorRating.end());
  }
}
```

This code is a little more complex ! It is heavily using the standard library of C++. By using this combination of [std::vector::erase](https://en.cppreference.com/w/cpp/container/vector/erase) and [std::remove_if](https://en.cppreference.com/w/cpp/algorithm/remove), we are able to remove the elements we don't want. By specifying a condition in them, with a [lambda](https://en.cppreference.com/w/cpp/language/lambda) we can specify the condition of removal which depend of the `rate` and and the bit which is mostly represented at this position in the lines of the binary message.

Once we exit the `for` loop, (meaning that we have iterated through all the letters), we should only have the rates available in the lists:
```cpp
assert(potentialsCo2ScrubberRating.size() == 1);
assert(potentialsOxygenGeneratorRating.size() == 1);
```

So we convert them to decimal:
```cpp
size_t binaryToDecimal(const std::string_view binary)
{
    auto decimalNumber{0};
    for(auto index = 0; index < binary.size(); ++index)
    {
        const auto& figure = binary[index];
        const auto powerOfTwo = binary.size() - 1 - index;
        if(figure == '1') { decimalNumber += std::pow(2, powerOfTwo); }
    }
    return decimalNumber;
}

/* ... */

oxygenGeneratorRating = binaryToDecimal(potentialsOxygenGeneratorRating.front());
co2ScrubberRating = binaryToDecimal(potentialsCo2ScrubberRating.front());
```

And finally, we display the solution:
```cpp
std::cout << "The solution is: " << (oxygenGeneratorRating * co2ScrubberRating) << std::endl;
```

<details>
  <summary>[Spoiler] Problem Answer</summary>

The puzzle answer was 5941884.
</details>

## Conclusion

You can note that the solutions written in this post, don't include all the sources to make running programs, but only the interesting part of the sources to solve this problem.
If you want to see the programs from end to end, you can go on my [GitHub account](https://github.com/Xav83/adventofcode2021/tree/main/Day%203), explore the full solution, add comments or ask questions if you want to, on the platform you read this article, it will also help me improve the quality of my articles.

--------------

Thank you all for reading this article,
And until my next article, have a splendid day 😉

![Advent Of Code 2021](https://raw.githubusercontent.com/Xav83/Xav83.github.io/master/res/Advent%20Of%20Code/2021/Screenshot%20Day%203.png)

## Interesting links

- [continue documentation](https://en.cppreference.com/w/cpp/language/continue)
- [constexpr documentation](https://en.cppreference.com/w/cpp/language/constexpr)
- [std::array documentation](https://en.cppreference.com/w/cpp/container/array)
- [std::vector documentation](https://en.cppreference.com/w/cpp/container/vector)
- [std::string_view documentation](https://en.cppreference.com/w/cpp/string/basic_string_view)
- [std::pow documentation](https://en.cppreference.com/w/cpp/numeric/math/pow)
- [std::vector::erase](https://en.cppreference.com/w/cpp/container/vector/erase)
- [std::remove_if](https://en.cppreference.com/w/cpp/algorithm/remove)
- [lambda](https://en.cppreference.com/w/cpp/language/lambda)
- [range-based for loop documentation](https://en.cppreference.com/w/cpp/language/range-for)
- [Advent of Code - Day 3](https://adventofcode.com/2021/day/3)
- [10xlearner website](www.10xlearner.com)
