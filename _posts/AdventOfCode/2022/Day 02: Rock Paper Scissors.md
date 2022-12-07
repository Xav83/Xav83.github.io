# AOC 2022 Day 2: Rock Paper Scissors

Hi there ! I'm Xavier Jouvenot and today we are continuing with the second [Advent Of Code](https://adventofcode.com) puzzle of the year 2022. 🦌

We are going to tackle the problem from the 2nd December 2022 (as expected), named "Rock Paper Scissors".
The solution I will propose in c++, but the reasoning can be applied to other languages.

_Self promotion_: You can find other articles on computer science and programming on my [website](www.10xlearner.com) 😉

## Part 1

### The Problem

The full version of this problem can be found directly on the [Advent of Code website](https://adventofcode.com/2022/day/2), I will only describe the essence of the problem here:

To reward us for the [solution found yesterday](https://10xlearner.com/2022/12/06/aoc-2022-day-1-calorie-counting/), the elves want to help us get the best tent (next to the snack storage), which is attributed via a giant Rock Paper Scissors tournament. They gave us the strategy to win the tournament without winning all the match (which would be suspicious), which look like the following:

```txt
A Y
B X
C Z
```

, with `A`, `B` and `C` meaning `Rock`, `Paper` and `Scissors` played by the elf facing us, and `X`, `Y` and `Z` also meaning `Rock`, `Paper` and `Scissors` but played by us.

The issue is that we don't know if the elves are trying to trick us, so we have to check beforehand if the strategy is actually a winning one. The only information we need is to know how the score of this tournament is calculated:
- for each round
   - if we choose `Rock`, we gain `1` point
   - if we choose `Paper`, we gain `2` points
   - if we choose `Scissors`, we gain `3` points
   - if we lose, we gain `0` points
   - if we draw, we gain `3` points
   - if we win, we gain `6` points

### Solution

Here is the complete solution, so that you can get a feel about it, but don't worry, we are going to explain it, step by step 😉

```cpp
using Hand = char;
using Score = size_t;

static constexpr Score LOST_SCORE = 0;
static constexpr Score DRAW_SCORE = 3;
static constexpr Score WIN_SCORE = 6;

static constexpr Score ROCK_SCORE = 1;
static constexpr Score PAPER_SCORE = 2;
static constexpr Score SCISSORS_SCORE = 3;

struct Match {
 Hand elf;
 Hand me;
 Score value;
};

constexpr std::array<Match, 9> matches{
   Match{'A', 'X', ROCK_SCORE + DRAW_SCORE},
   Match{'A', 'Y', PAPER_SCORE + WIN_SCORE},
   Match{'A', 'Z', SCISSORS_SCORE + LOST_SCORE},
   Match{'B', 'X', ROCK_SCORE + LOST_SCORE},
   Match{'B', 'Y', PAPER_SCORE + DRAW_SCORE},
   Match{'B', 'Z', SCISSORS_SCORE + WIN_SCORE},
   Match{'C', 'X', ROCK_SCORE + WIN_SCORE},
   Match{'C', 'Y', PAPER_SCORE + LOST_SCORE},
   Match{'C', 'Z', SCISSORS_SCORE + DRAW_SCORE}};

PuzzleSolution computeFirstPartSolution(const std::string_view input) {
 Score finalScore{0};
 for (auto it = input.begin(); it != input.end(); it = std::next(it, 4)) {
   assert(std::distance(it, std::find(it, input.end(), '\n')) == 3);

   const Hand elf = *it;
   const Hand me = *std::next(it, 2);

   auto currentMatch = std::find_if(
       matches.begin(), matches.end(), [&elf, &me](const auto &match) {
         return match.elf == elf and match.me == me;
       });
   assert(currentMatch != matches.end());
   finalScore += currentMatch->value;
 }
 return finalScore;
}
```

First of all, we start by creating some useful constants, and a `Match` structure which we are going to use for the actual algorithm.

Then, we create an array named `matches` with all the rounds possible. Since there are only 9 combinations possible in a "Rock Paper Scissors" game, it was pretty easy to do by hand. And now, we have an array to refer to to know the score of each round the elves want us to play.

Finally, we arrive at the algorithm computing the result of the tournament! In this algorithm, we go through each round, to find the corresponding score of this round in our `matches` variable, and add the score found to the `finalScore` variable. Once we finish going through the input, the variable `finalScore` will contain, as the name suggest, the result we are looking for 😉

## Part 2

### The Problem

Now, the elves tell us that the `X`, `Y` and `Z` letters actually have a different meaning. `X` means that we have to lose, `Y` means that we have to end the round in a draw, and `Z` means we need to win.

### Solution

Well, with everything we have set up in the previous part, all we have to do is to generate a new `matches` variable which integrates the new information given by the elves:

```cpp
constexpr std::array<Match, 9> matches{
   Match{'A', 'X', SCISSORS_SCORE + LOST_SCORE},
   Match{'A', 'Y', ROCK_SCORE + DRAW_SCORE},
   Match{'A', 'Z', PAPER_SCORE + WIN_SCORE},
   Match{'B', 'X', ROCK_SCORE + LOST_SCORE},
   Match{'B', 'Y', PAPER_SCORE + DRAW_SCORE},
   Match{'B', 'Z', SCISSORS_SCORE + WIN_SCORE},
   Match{'C', 'X', PAPER_SCORE + LOST_SCORE},
   Match{'C', 'Y', SCISSORS_SCORE + DRAW_SCORE},
   Match{'C', 'Z', ROCK_SCORE + WIN_SCORE}};
```

And voilà, we compile again to obtain our result for the tournament 😉

## Conclusion

Today's problem was really interesting, I had to think a little bit in order to know how to apprehend and solve it.
Of course, there are probably more optimal solution to this problem, so feel free to comment with yours 😉

You can note that the solutions written in this post, don't include all the sources to make running programs, but only the interesting part of the sources to solve this problem.

If you want to see the programs from end to end, you can go on my [GitHub account](https://github.com/Xav83/advent_of_code/tree/main), explore the full solution, add comments or ask questions if you want to, on the platform you read this article, it will also help me improve the quality of my articles.

---------------

Thank you all for reading this article,
And until my next article, have a terrific day 😉

## Interesting links

- [Advent of Code website](https://adventofcode.com/2022)
- C++ keywords: [using](https://en.cppreference.com/w/cpp/keyword/using), [constexpr](https://en.cppreference.com/w/cpp/language/constexpr) and [static](https://en.cppreference.com/w/cpp/language/storage_duration)
- C++ functions: [std::find](https://en.cppreference.com/w/cpp/algorithm/find), [std::next](https://en.cppreference.com/w/cpp/iterator/next) and [std::distance](https://en.cppreference.com/w/cpp/iterator/distance)
- C++ structure: [std::array](https://en.cppreference.com/w/cpp/container/array) and [std::string_view](https://en.cppreference.com/w/cpp/string/basic_string_view)
