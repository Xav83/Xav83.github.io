# Advent Of Code 2021 - The Treachery of Whales - Puzzle 7

Hello ! I'm Xavier Jouvenot and here is the 7th part of a long series on [Advent Of Code 2021](https://adventofcode.com).

For this new post, we are going to solve the problem from the 7th December 2021, named "The Treachery of Whales".
The solution I will propose in C++, but the reasoning can be applied to other languages.

_Self promotion_:
Here are a few social networks where you can follow me and check my work as a programmer and a writer 😉 
[personal website](www.10xlearner.com), [Twitter](https://twitter.com/10xLearner), [Dev.to](https://dev.to/10xlearner), [CodeNewbie](https://community.codenewbie.org/xav83), [Medium](https://medium.com/@xavier-jouvenot), [GitHub](https://github.com/Xav83/)

## Part 1

### The Problem

The full version of this problem can be found directly on the [Advent of Code website](https://adventofcode.com/2021/day/7), I will only describe the essence of the problem here:

We are trying to escape a giant whale and a swarm of crabs (each in its own tiny submarine) come to rescue us. To do so, they are going to blast a hole in the ocean floor to open an underground cave, but they need to be aligned on the horizontal axis to do so. We can help them to aligned as fast as possible by giving them the horizontal position to which they all can move without burning too much fuel. To complete that goal, we have the position of each crab submarine on the horizontal axis, something like:
```
16,1,2,0,4,2,7,1,2,14
``` 
, and we know that moving from 1 unit on the horizontal axis costs 1 unit of fuel. As a result for the problem to be completely solved, we need to fund out the amount of fuel consumed by all the crabs' submarine to reach this optimal position.

### Solution

With a bit of object oriented programming and the standard library, you'll see that we can come up with a pretty straight forward solution.
First we need to create a `Crab` structure (maybe I should have named it "CrabSubmarine" 🤷)

```cpp
using HorizontalPosition = int;

class Crab
{
public:
    Crab(HorizontalPosition initialPosition_) : initialPosition(initialPosition_) {}

    int howFarFrom(HorizontalPosition otherPosition) const { return std::abs(initialPosition - otherPosition); }
    int getPosition() const { return initialPosition; }

    bool operator<(const Crab& other) const { return initialPosition < other.initialPosition; }

private:
    HorizontalPosition initialPosition{0};
};
```

So here, we have a class which stores the horizontal position of one crab's submarine and can tell us **how far from** another position it is.
We also have an operator `<` defined to compare easily the Crab horizontal position, which will come handy later 😉

Now, using this class, let's look at the code solving the problem at hand
```cpp
const std::array<Crab, 1000> input_crab_position {1101,1,29,67, /* */ 483,1451};

int main()
{
    auto [min, max] = std::minmax_element(std::begin(input_crab_position), std::end(input_crab_position));
    auto minimumFuelFound = std::numeric_limits<int>::max();
    for(auto potentialBestPosition=min->getPosition(); potentialBestPosition<max->getPosition(); ++potentialBestPosition)
    {
        auto fuelUsed = std::accumulate(std::begin(input_crab_position), std::end(input_crab_position), 0,
                                        [&potentialBestPosition](const auto& sum, const auto& crab){ return sum + crab.howFarFrom(potentialBestPosition); });
        minimumFuelFound = std::min(minimumFuelFound, fuelUsed);
    }

    std::cout << "The solution is: " << minimumFuelFound << std::endl;
    return 0;
}
```
In plain English this code starts by getting the range where the crabs are, using [std::minmax_element](https://en.cppreference.com/w/cpp/algorithm/minmax_element), then calculate, for each position in this range, the amount of fuel that all the crabs' submarines will consume if it was the optimal position with the [std::accumulate call](https://en.cppreference.com/w/cpp/algorithm/accumulate) and finally, check it the amount calculated is the smaller found so far and, if so, puts the value in the variable `minimumFuelFound` to be printed once all the positions will have been checked.

This solution has no clever trick, this is what is called a [Brute-force search](https://en.wikipedia.org/wiki/Brute-force_search) as we are computing all the potential solutions in order to find the right one. For some problems, as in the [previous one](https://10xlearner.com/2022/02/08/advent-of-code-2021-lanterfish-puzzle-6/), this can cause some issues (the solution being to long to compute, for example), but in the case of this problem, the solution is render almost instantaneously.

<details>
  <summary>[Spoiler] Problem Answer</summary>

The puzzle answer was 336701.
</details>

## Part 2

### Problem

For this part of the problem, it comes to our knowledge that the crabs' submarine don't consume fuel linearly, but for each unit they move, they consume 1 unit of fuel more than during their previous move, for example: moving 1 unit, they consume 1 unit of fuel, but to move 2 units, they will consume 3 units of fuel (1+2), and to move 3 units, they will consume 6 units of fuel (1+2+3), and so on.

### Solution

I order to solve this part of the problem, I still used a [Brute-force search](https://en.wikipedia.org/wiki/Brute-force_search), as it is easy code to write !
All I had to do was adding a new method in the Crab class, and modify one line in the main function:
```cpp
class Crab {
    /*...*/
    int howFarFrom_2(HorizontalPosition otherPosition) const
    {
        auto totalFuel{0};
        auto rawDistance = howFarFrom(otherPosition);
        for(auto i=rawDistance; i>0; --i)
        {
            totalFuel += i;
        }
        return totalFuel;
    }
    /*...*/
};
``` 

This function is the new calculation of the fuel consumption of a crab's submarine.
It is the code translation of the calculation method described in the problem text, so we iterate over the distance we want to travel and we increment the amount of fuel used each step of the way.

Then, instead of calling the `howFarFrom` method in the lambda of the [std::accumulate call](https://en.cppreference.com/w/cpp/algorithm/accumulate), we call this new function.
```cpp
auto fuelUsed = std::accumulate(std::begin(input_crab_position), std::end(input_crab_position), 0,
                                [&potentialBestPosition](const auto& sum, const auto& crab){ return sum + crab.howFarFrom_2(potentialBestPosition); });
```

<details>
  <summary>[Spoiler] Problem Answer</summary>

The puzzle answer was 95167302s.
</details>

Even if it takes more time that the previous part of this day's problem to be computed, we have the solution displayed in about 4 seconds in our terminal 😉

#### And that's it ?

For me, yes, I found the solution and I am happy with the solution I have got.
At least, I was until I went to look at what other people have done on [Reddit](https://www.reddit.com/r/adventofcode/search/?q=Day%207&restrict_sr=1&sr_nsfw=)!

At the moment, it didn't come to me that we could go one step (or many steps) further to solve this problem more efficiently, I didn't felt the need to do it. But some people did, and I found that very interesting 🙂
So I am going to give you some links to their work, so you too can see some great ways to solve this problem !

The first one that I want to mention is a paper written by *throwaway7824365346*, [available on Reddit](https://www.reddit.com/r/adventofcode/comments/rawxad/2021_day_7_part_2_i_wrote_a_paper_on_todays/) describing how to mathematically calculated the solution in a very efficient way. It is a very interesting read, I really enjoyed it.

The second one I found really interesting is a [video](https://www.youtube.com/watch?v=XvoCyVFQOm8) *Goatfreed* which used [GitHub Copilot](https://copilot.github.com/) to generate some typescript code that can compute the solution of this day's problem. Really impressive !

Finally, I just want to point out this [Reddit thread](https://www.reddit.com/r/adventofcode/comments/raz02s/2021_day_7_im_just_happy_my_gigantic_list/) which summarize my reaction when looking at others people solution and some other approach to the problem that other people have used.

## Conclusion

You can note that the solutions written in this post, don't include all the sources to make running programs, but only the interesting part of the sources to solve this problem.
If you want to see the programs from end to end, you can go on my [GitHub account](https://github.com/Xav83/adventofcode2021/tree/main/Day%207), explore the full solution, add comments or ask questions if you want to, on the platform you read this article, it will also help me improve the quality of my articles.

--------------

Thank you all for reading this article,
And until my next article, have a splendid day 😉

![Advent Of Code 2021](https://raw.githubusercontent.com/Xav83/Xav83.github.io/master/res/Advent%20Of%20Code/2021/Screenshot%20Day%207.png)

## Interesting links

- [Brute-force search definition](https://en.wikipedia.org/wiki/Brute-force_search)
- [constexpr documentation](https://en.cppreference.com/w/cpp/language/constexpr)
- [std::array documentation](https://en.cppreference.com/w/cpp/container/array)
- [std::accumulate documentation](https://en.cppreference.com/w/cpp/algorithm/accumulate)
- [std::minmax_element documentation](https://en.cppreference.com/w/cpp/algorithm/minmax_element)
- [lambda documentation](https://en.cppreference.com/w/cpp/language/lambda)
- [range-based for loop documentation](https://en.cppreference.com/w/cpp/language/range-for)
- [Solution by throwaway7824365346](https://www.reddit.com/r/adventofcode/comments/rawxad/2021_day_7_part_2_i_wrote_a_paper_on_todays/)]
- [Youtube video by Goatfreed](https://www.youtube.com/watch?v=XvoCyVFQOm8)
- [Advent of Code - Day 7](https://adventofcode.com/2021/day/7)
- [10xlearner website](www.10xlearner.com), [Twitter](https://twitter.com/10xLearner), [Dev.to](https://dev.to/10xlearner), [CodeNewbie](https://community.codenewbie.org/xav83), [Medium](https://medium.com/@xavier-jouvenot), [GitHub](https://github.com/Xav83/)
