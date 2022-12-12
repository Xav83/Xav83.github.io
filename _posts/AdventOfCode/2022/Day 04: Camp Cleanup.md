# AOC 2022 Day 4: Camp Cleanup

Hi there ! I'm Xavier Jouvenot and today we are continuing with the fourth [Advent Of Code](https://adventofcode.com) puzzle of the year 2022. 🌟

We are going to tackle the problem from the 4th December 2022, named "Camp Cleanup".
The solution I will propose in c++, but the reasoning can be applied to other languages.

_Self promotion_: You can find other articles on computer science and programming on my [website](www.10xlearner.com) 😉

## Part 1

### The Problem

The full version of this problem can be found directly on the [Advent of Code website](https://adventofcode.com/2022/day/4), I will only describe the essence of the problem here:

By pairs, the elves are cleaning the camp, and need our help since they realized that the plan they use to clean the camp has some errors. Indeed, each member of the pair of elves has some section to clean, but, it happens that one elf has to clean a section which is fully inside the section that the other elf has to clean, meaning that the work is done twice.

In order for you to know what we are dealing with, here is an example of small plan:
```txt
2-4,6-8
2-3,4-5
5-7,7-9
2-8,3-7
6-6,4-6
2-6,4-8
```

, with each line representing the sections assigned to a pair of elves. For example, `2-4,6-8` means that the first elf has to clean the camp from the section `2` to section `4`, and the second one has to clean the camp from the section `6` to the section `8`.

### Solution

Here is the complete solution, so that you can get a feel about it, but don't worry, we are going to explain it, step by step 😉

```cpp
using SectionID = size_t;

constexpr char SECTION_DELIMITER = ',';
constexpr char RANGE_SEPARATOR = '-';

struct Section {
 [[nodiscard]] bool isContainedIn(const Section &other) const {
   return other.begin <= begin && end <= other.end;
 }

 SectionID begin{0};
 SectionID end{0};
};

PuzzleSolution computeSolution(const std::string_view input) {
 PuzzleSolution pairsOfSectionsContainingOneAnother{0};

 for (auto it = input.begin(); it != input.end();) {
   const auto endOfLine = std::find(it, input.end(), '\n');
   const auto sectionDelimiterIt = std::find(it, endOfLine, SECTION_DELIMITER);
   const auto firstRangeSeparatorIt =
       std::find(it, sectionDelimiterIt, RANGE_SEPARATOR);
   const auto secondRangeSeparatorIt =
       std::find(sectionDelimiterIt, endOfLine, RANGE_SEPARATOR);

   const auto firstNumberOfFirstRange =
       std::string_view(it, firstRangeSeparatorIt);
   const auto secondNumberOfFirstRange =
       std::string_view(std::next(firstRangeSeparatorIt), sectionDelimiterIt);
   const auto firstNumberOfSecondRange =
       std::string_view(std::next(sectionDelimiterIt), secondRangeSeparatorIt);
   const auto secondNumberOfSecondRange =
       std::string_view(std::next(secondRangeSeparatorIt), endOfLine);

   const Section firstSection(std::atoi(firstNumberOfFirstRange.data()),
                              std::atoi(secondNumberOfFirstRange.data()));
   const Section secondSection(std::atoi(firstNumberOfSecondRange.data()),
                               std::atoi(secondNumberOfSecondRange.data()));

   if (firstSection.isContainedIn(secondSection) ||
       secondSection.isContainedIn(firstSection)) {
     ++pairsOfSectionsContainingOneAnother;
   }

   it = std::next(endOfLine);
 }
 return pairsOfSectionsContainingOneAnother;
}
```

So, as usual, we start by creating a few useful aliases and constant variables. We also create a structure `Section` with a function `isContainedIn` which is going to make the rest of the code more readable.

Then, in the main function `computeSolution`, I iterate through the input line by line, and, on each line, I extract the information we need using [std::find](https://en.cppreference.com/w/cpp/algorithm/find), [std::next](https://en.cppreference.com/w/cpp/iterator/next), and [std::string_view](https://en.cppreference.com/w/cpp/string/basic_string_view), and use [std::atoi](https://en.cppreference.com/w/cpp/string/byte/atoi) in order to convert the `string_view` into numbers to create the `Section` variables. With them, we can easily check if one of the sections is contained in the other one, and increment our result variable.

## Part 2

### The Problem

Now, the elves tell us that they actually want to know when the sections overlap, to reduce the number of duplicated work.

### Solution

For this part, all we have to do is to create a new function `doesOverlapWith`, and call it instead of `isContainedIn` from the previous solution.

```cpp
struct Section {
 [[nodiscard]] bool doesOverlapWith(const Section &other) const {
   return other.begin <= begin && begin <= other.end ||
          other.begin <= end && end <= other.end;
 }

 /// rest of the struct code
};
```

And voilà 🙂

## Conclusion

I've reflected a lot on the choices I made to solve this problem when writing this blog post, and this is probably not the most sexy solution for this problem. Indeed, I have solved it in a naive way, with the standard functions, and it works, but I could probably have used some other techniques to do it. On the top of my head, I can very well imagine having an algorithm parsing the input, character by character, and using a state machine to solve this puzzle really fast. Or using regex in order to extract the information needed in an interesting way. If you have such a solution, feel free to dm it to me 😉

If you want to see the programs from end to end, you can go on my [GitHub account](https://github.com/Xav83/advent_of_code/tree/main), explore the full solution, add comments or ask questions if you want to, on the platform you read this article, it will also help me improve the quality of my articles.

---------------

Thank you all for reading this article,
And until my next article, have a wonderful day 😉

## Interesting links

- [Advent of Code website](https://adventofcode.com/2022)
- C++ keywords: [char](https://en.cppreference.com/w/cpp/keyword/char) and [using](https://en.cppreference.com/w/cpp/language/type_alias)
- C++ functions: [std::find](https://en.cppreference.com/w/cpp/algorithm/find), [std::next](https://en.cppreference.com/w/cpp/iterator/next) and [std::atoi](https://en.cppreference.com/w/cpp/string/byte/atoi)
- C++ structures: [std::string_view](https://en.cppreference.com/w/cpp/string/basic_string_view)
