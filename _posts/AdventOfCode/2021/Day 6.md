# Advent Of Code 2021 - Lanternfish - Puzzle 6

Hello ! I'm Xavier Jouvenot and here is the 6th part of a long series on [Advent Of Code 2021](https://adventofcode.com).

For this new post, we are going to solve the problem from the 6th December 2021, named "Lanternfish".
The solution I will propose in C++, but the reasoning can be applied to other languages.

_Self promotion_:
Here are a few social networks where you can follow me and check my work as a programmer and a writer 😉 
[personal website](www.10xlearner.com), [Twitter](https://twitter.com/10xLearner), [Dev.to](https://dev.to/10xlearner), [CodeNewbie](https://community.codenewbie.org/xav83), [Medium](https://medium.com/@xavier-jouvenot), [GitHub](https://github.com/Xav83/)

## Part 1

### The Problem

The full version of this problem can be found directly on the [Advent of Code website](https://adventofcode.com/2021/day/6), I will only describe the essence of the problem here:

While diving with our submarine, we encounter some lantern fish and see that they spawn quickly, so for some reason, we decide to how fast they are growing in number. By chance, we happen to know that each lantern fish creates a new lantern fish once every 7 days, but new lantern fish takes a little longer, on they first cycle to procreate : 9 days. Finally, the submarine is able to give us a list of the age of each lantern fish surrounding us.
Something like:
```
3,4,3,1,2
```
where each number represents the time before one particular lantern fish will create a new one.

For this part of the day's problem, we want to know how many lantern fish there is going to be after **80** days.

### Solution

For this part of the day's problem, we can come up with a very naive coding solution, following step by step the lantern fish procreation cycle, to find the result wanted. 😊
To start, we create a lantern fish class, with some helpful functions:
```cpp
class LanternFish
{
public:
    constexpr LanternFish() = default;
    constexpr LanternFish(int timeLeftBeforeDuplicating_) : timeLeftBeforeDuplicating(timeLeftBeforeDuplicating_) {}

    constexpr void gettingOlder()
    {
        if(isGoingToDuplicate ())
        {
            timeLeftBeforeDuplicating = 6;
            return;
        }
        --timeLeftBeforeDuplicating;
    }

    constexpr bool isGoingToDuplicate () const { return timeLeftBeforeDuplicating == 0 ; }

private:
    int timeLeftBeforeDuplicating{8};
};

constexpr std::array<LanternFish, 300> input_lines {3,3,2, /* ... */,4};
```

In this class, we have a counter which track the number of days left before the reproduction of one lantern fish. Once it reaches `0`, it means that the lantern fish is going to create a new one and its own cycle start again at `6`.

Then, we add 2 functions which are going to make the main code simpler to write and read:
```cpp
int getNumberOfLanterFishToBeBorn(const std::vector<LanternFish>& fishes) {
    return std::count_if(std::begin(fishes), std::end(fishes),
                         [](const auto& fish){ return fish.isGoingToDuplicate(); });
}

void fishesGettingOlder (std::vector<LanternFish>& fishes) {
    for(auto& fish : fishes) { fish.gettingOlder(); }
}
```
The first one gives us the number of fish that are going to spawn, by counting the number of lantern fish at the end of their cycle while the second one make the existing fish grow one day older.

Finally, we have all the tool necessary to compute the solution in a few line:
```cpp
std::vector<LanternFish> fishes(input_lines.begin(), input_lines.end());
for(auto i=0; i<80; ++i) {
    const auto numberOfNewBorn = getNumberOfLanterFishToBeBorn(fishes);
    fishesGettingOlder(fishes);
    for(auto j=0; j<numberOfNewBorn; ++j) { fishes.emplace_back(); }
}

std::cout << "The solution is: " << fishes.size() << std::endl;
```

So for each day, we get the number of lantern fish to be born, we make the current lantern fish get one day older, and we add the newborn into the pool of fishes. After repeating this process 80 times, we have the result we are looking for. 😉

<details>
  <summary>[Spoiler] Problem Answer</summary>

The puzzle answer was 380758.
</details>

## Part 2

### Problem

For the second part, we want to extend the duration of the prevision from **80** days to **256** days.

### Solution

At first, when I read the part of the problem, I was like: "Really ??! All I have to do is to change the number `80` in the for loop into `256` and let the computer process !". I though I was so clever... Sooo after about 15 minutes, the program ended with this gentle message:
```
terminate called after throwing an instance of 'std::bad_alloc'
  what():  std::bad_alloc
Aborted (core dumped)
```
This meant that the element `std::vector<LanternFish> fishes` became so big that it literally couldn't accept one more fish in it.
Humble by that result, I took a break and though about how I could apprehend this problem from another angle.

After a good shower, I was able to come up with a solution revolving around this new element `std::array<size_t, 9> sortedFishes{0};`!
Let me explain 😉
Each lantern fish have a cycle of maximum 9 days. So instead of modeling each lantern fish, we can group them depending on their position in their reproduction cycle, and track the 9 groups through time, instead of the innumerable lantern fish individually.

So the first thing we do is to create the group of lantern fish:
```cpp
std::array<size_t, 9> sortedFishes{0};
for(auto i=0; i<sortedFishes.size(); ++i) {
    sortedFishes[i] = std::count_if(std::begin(input_lines), std::end(input_lines),
                                    [i](const auto& fish){ return fish.getTimeLeftBeforeDuplicating () == i; });
}
```

The index of the fish in the array represent the number of days left before it duplicates.
Now that we have all the fishes grouped, we let the days pass:
```cpp
for(auto i=0; i<256; ++i)
{
    std::array<size_t, 9> nextDaySortedFishes{0};
    nextDaySortedFishes[6] += sortedFishes.front();
    nextDaySortedFishes[8] += sortedFishes.front();
    for(auto i=1; i<sortedFishes.size(); ++i)
    {
        nextDaySortedFishes[i-1] += sortedFishes[i];
    }
    sortedFishes = nextDaySortedFishes;
}
```
In this code, we iterate through the **256** days, and for each day, we create a new array to put the lantern fish in their new group, as they are growing older. The lantern fish which are going to duplicate are put in the `6`th group and as many lantern fish are put in the `8`th group. Then, for all the other lantern fish, we put them in the group with an index under their current one. Finally, we copy our new distribution in our main element to keep our result from one day to the other.

Finally, we sum the number of fish in each group at the end of the **256** days, and get the result we wanted:
```cpp
const auto numberOfFishes = std::accumulate(std::begin(sortedFishes), std::end(sortedFishes), size_t{0});
std::cout << "The solution is: " << numberOfFishes << std::endl;
```

The `size_t{0}` is very important ! Since if you use an integer, then, you will end up with an overflow and won't have the right answer at the end of the program!

<details>
  <summary>[Spoiler] Problem Answer</summary>

The puzzle answer was 1710623015163.
</details>

## Conclusion

This problem showed me how easy it is to underestimate the difficulty of a problem, and how fast we can encounter some limitations (from our machine, or our mind) when dealing with issues to solve. A really humbling experience !

After solving the problem on my own, I went on the Reddit of Advent Of Code, and looked at what people have done to tackle this problem, and I found this [article from maciejkedzior](https://maciejkedzior.github.io/update/2022/01/03/advent-of-code-2021-day-6.html) who actually came with a similar way to solve the problem (in C, not C++), which I found funny (I am a simple man 😂). I then wonder on his GitHub and his blog, he seems like a really nice fellow, so you can check his work on [here](https://maciejkedzior.github.io/update/2022/01/03/welcome-on-my-blog.html) 😉

You can note that the solutions written in this post, don't include all the sources to make running programs, but only the interesting part of the sources to solve this problem.
If you want to see the programs from end to end, you can go on my [GitHub account](https://github.com/Xav83/adventofcode2021/tree/main/Day%206), explore the full solution, add comments or ask questions if you want to, on the platform you read this article, it will also help me improve the quality of my articles.

--------------

Thank you all for reading this article,
And until my next article, have a splendid day 😉

![Advent Of Code 2021](https://raw.githubusercontent.com/Xav83/Xav83.github.io/master/res/Advent%20Of%20Code/2021/Screenshot%20Day%206.png)

## Interesting links

- [constexpr documentation](https://en.cppreference.com/w/cpp/language/constexpr)
- [std::array documentation](https://en.cppreference.com/w/cpp/container/array)
- [C++ vector max size](https://stackoverflow.com/questions/3813124/c-vector-max-size)
- [std::count_if documentation](https://en.cppreference.com/w/cpp/algorithm/count)
- [std::accumulate documentation](https://en.cppreference.com/w/cpp/algorithm/accumulate)
- [lambda](https://en.cppreference.com/w/cpp/language/lambda)
- [range-based for loop documentation](https://en.cppreference.com/w/cpp/language/range-for)
- [article from maciejkedzior](https://maciejkedzior.github.io/update/2022/01/03/advent-of-code-2021-day-6.html)
- [Advent of Code - Day 6](https://adventofcode.com/2021/day/6)
- [10xlearner website](www.10xlearner.com), [Twitter](https://twitter.com/10xLearner), [Dev.to](https://dev.to/10xlearner), [CodeNewbie](https://community.codenewbie.org/xav83), [Medium](https://medium.com/@xavier-jouvenot), [GitHub](https://github.com/Xav83/)
