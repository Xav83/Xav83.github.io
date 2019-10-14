# Advent Of Code - Reindeer Olympics - Puzzle 14

Hello ! I'm Xavier Jouvenot and here is the part fourteen of a long series on [Advent Of Code](https://adventofcode.com). You can find the previous part [here](2019-10-14-AdventOfCode2015-Day14-Reindeer-Olympics.md)

For this new post, we are going to solve the problem from the 14th December 2015, named "Reindeer Olympics".
The solution I will propose in C++, but the reasoning can be applied to other languages.

## Part 1

### The Problem

The full version of this problem can be found directly on the [Advent of Code website](https://adventofcode.com/2015/day/14), I will only describe the essence of the problem here:

It's the Reindeer Olympics ! A race between Santa's reindeer ! I would love to witness such event ! 🎅

The reindeer can fly only for several seconds before needing some rest.
For example, we can have a reindeer named Comet which can fly `14 km/s` for `10` seconds, but then must rest for `127` seconds.

After one second, Comet has gone 14 km. After ten seconds, Comet has gone 140 km. On the eleventh second, Comet begins resting (staying at 140 km) for 127 seconds.

For this race, **we will have to find what distance the winning reindeer run through**, knowing that the race will last exactly 2503 seconds (because all races should last 2503 seconds 😆).

### Solution

First thing first, let's define what is going to be a Reindeer in our program.

```c++
class Reindeer
{
public:
    using Speed = size_t;
    using Position = size_t;
    using Time = size_t;

    Reindeer (const std::string& name_, const Speed flyingSpeed_, const Time flyingTime_, const Time restTime_);
    ~Reindeer ();

    void oneSecondPassed ();
    Position getCurrentPosition () const;

private:
    std::string name;
    Speed flyingSpeed{0};
    Time flyingTime{0};
    Time restTime{0};

    Position position{0};
    Time timeBeforeChangingState{0};
    bool isFlying{true};
};
```

Wow, that's a lot of variables ! But we are going to need all of them (except of the name, that I only used for debug purpose).
The variables `flyingSpeed`, `flyingTime` and `restTime` are what the characteristics of the Reindeer for the Race.
The variable `position` is the one which is going to help us found out which of the Reindeer win the race, and the two last variables (`timeBeforeChangingState` and `isFlying`) are going to help us figuring out when the reindeer can fly and when it must rest. So let's take a look at how their are used :

```c++
void Reindeer::oneSecondPassed ()
{
    if(timeBeforeChangingState == 0)
    {
        isFlying = !isFlying;
        timeBeforeChangingState = isFlying ? flyingTime : restTime;
    }

    if(isFlying)
    {
        position += flyingSpeed;
    }
    --timeBeforeChangingState;
}
```

How does it work ?!

The variable `timeBeforeChangingState` tracks the time the reindeer still has either to fly or to rest.
So when it reaches `0`, we change state and reset `timeBeforeChangingState` with the time variable matching the new state.

And we also updates the position if the reindeer is currently flying.

So now that we know what a reindeer looks like, and how they behave at each second; making them go through the race will be pretty straight forwards.
Indeed, since the race lasts `2503` seconds, we only need to use for loops:

```c++
for(auto i = 0; i < 2503; ++i)
{
    for(auto& reindeer: reindeers)
    {
        reindeer.oneSecondPassed();
    }
}
```

But before run the race, we have to know who are the participants of the race.
For information, a reindeer description look like that : "Prancer can fly 18 km/s for 6 seconds, but then must rest for 103 seconds."
So to extract the useful information to make a instance of a Reindeer.

```c++
Reindeer extractInformationFrom (const std::string& line)
{
    std::istringstream stream(line);
    std::string reindeerName, can, fly, km_s, for_, seconds, but, then, must, rest, for__, seconds_;
    Reindeer::Speed flyingSpeed;
    Reindeer::Time flyingTime;
    Reindeer::Time restTime;
 
    stream >> reindeerName >> can >> fly >> flyingSpeed >> km_s >> for_ >> flyingTime >> seconds >> but >> then >> must >> rest >> for__ >> restTime >> seconds_;
    return Reindeer(reindeerName, flyingSpeed, flyingTime, restTime);
}
```

Inspired by a comment on my [last article](https://10xlearner.com/2019/10/07/advent-of-code-knights-of-the-dinner-table-puzzle-13/) on [Reddit](https://www.reddit.com/r/adventofcode/comments/cymboz/advent_of_code_all_in_a_single_night_puzzle_9/), I have decided to use a `std::istringstream` to get each word in the line describing each reindeer.

The useless words will be deleted from the stack at the end of the function, while the useful one will serve to create the "Reindeer".

I found this solution pretty simple, I don't think I would have been able to think about such thing by myself, so thank you **algmyr** for your comment 😃
If you, who are reading this post, want to make a comment about some part of it, be my guest, I will gladly read them 🙂

Finally, for this first part, we must find the winner of the race !
To found it, all we need is to use the std function `std::max_element`, like that :

```c++
const auto winningReindeer =
    std::max_element(
        std::begin(reindeers),
        std::end(reindeers),
        [](const auto& firstReindeer, const auto& secondReindeer)
        {
            return firstReindeer.getCurrentPosition () < secondReindeer.getCurrentPosition ();
        });
const auto distanceOfWinningReindeer = winningReindeer->getCurrentPosition ();
```

And voilà, we have the distance runned by the winner of the race.

## Part 2

### Problem

When seeing the result of the race in the part one, Santa decides to change the scoring system.
So, instead of the old one, now, he gives one point to each reindeer leading the race at each second.

### Solution

So now, the race look a bit different:

```c++
for(auto i = 0; i < 2503; ++i)
{
    for(auto& reindeer: reindeers)
    {
        reindeer.oneSecondPassed();
    }

    const auto winningReindeer = std::max_element(std::begin(reindeers), std::end(reindeers), [](const auto& firstReindeer, const auto& secondReindeer){ return firstReindeer.getCurrentPosition () < secondReindeer.getCurrentPosition (); });
    const auto distanceOfWinningReindeer = winningReindeer->getCurrentPosition ();

    for(auto& reindeer: reindeers)
    {
        if(reindeer.getCurrentPosition() == distanceOfWinningReindeer)
        {
            reindeer.winBonusPoint();
        }
    }
}
```

So after each of the Reindeer make is move for the current second, we look at the leading reindeer, with std::max_element.
Once we have it, we look how far it is from the start, and we give one bonus point to each reindeer at the same distance from the start.

The function `Reindeer::winBonusPoint` only increase a new variable in the `Reindeer` class, to store the number of points of each reindeer.

And now, to know the winner, we use almost the same code as in the first part:

```c++
const auto winningReindeer =
    std::max_element
    std::begin(reindeers),
    std::end(reindeers),
    [](const auto& firstReindeer, const auto& secondReindeer)
    {
        return firstReindeer.getCurrentBonusPoint () < secondReindeer.getCurrentBonusPoint ();
    });
const auto pointsOfWinningReindeer = winningReindeer->getCurrentBonusPoint ();
```

And voilà, we know the number of points of the winner of the race 🤘

## Conclusion

You can note that the solutions, written in this post, don't include all the sources to make running programs, but only the interesting part of the sources to solve this problem.
If you want to see the programs from end to end, you can go on my [GitHub account](https://github.com/Xav83/AdventOfCode/tree/2015.14/2015/Day14), explore the full solution, add comments or ask questions if you want to, on the platform you read this article, it will also help me improve the quality of my articles.

Here is the list of elements that we have used, I can't encourage you enough to look at their definitions :

- [std::max_element](https://en.cppreference.com/w/cpp/regex)
- [std::begin](https://en.cppreference.com/w/cpp/iterator/begin)
- [std::end](https://en.cppreference.com/w/cpp/iterator/end)
- [std::istringstream](https://en.cppreference.com/w/cpp/io/basic_istringstream)
- [std::string](https://en.cppreference.com/w/cpp/string/basic_string)
- [using](https://en.cppreference.com/w/cpp/language/type_alias)

Thanks for you reading, hope you liked it 😃

And until next part, have fun learning and growing.
