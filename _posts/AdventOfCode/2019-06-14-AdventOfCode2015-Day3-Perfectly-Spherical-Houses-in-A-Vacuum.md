# Advent Of Code - Perfectly Spherical Houses in a Vacuum - Puzzle 3

Hello ! I'm Xavier Jouvenot and here is the third part of a long series on [Advent Of Code](https://adventofcode.com). You can find the previous part [here](2019-06-07-AdventOfCode2015-Day2-I-Was-Told-There-Would-Be-No-Math.md)

For this new post, we are going to solve the problem from the 3rd December 2015, named "Perfectly Spherical Houses in a Vacuum".
The solution I will propose in C++, but the reasoning can be applied to other languages.

## Part 1

### The Problem

The full version of this problem can be found directly on the [Advent of Code website](https://adventofcode.com/2015/day/3), I will only describe the essence of the problem here:

Santa is delivering presents to an infinite two-dimensional grid of houses.
He starts by delivering a present to the house at his starting location, and then an elf at the North Pole tells him, via radio where to move next. He moves one house at the time either to the north (`^`), south (`v`), east (`>`), or west (`<`). Each time, Santa visits a house, he delivers one present, but, the elf giving the instructions to Santa drank too much eggnog, and made Santa visiting houses more than once. So, we have to found out **how many houses receive at least one present**.

For example:
- `>>` makes Santa deliver one present to `3` houses, the one in which he starts and the 2 houses to the east from there.
- `^v^v^v^v^v` makes Santa deliver a bunch of presents to `2` houses.

### Solution

Here I will describe the thought process I've used to arrive to the solution.

First, we have to know the location of Santa on the grid of houses, which means we need his **Coordinate**.

```c++
struct Coordinate
{
    int x{0};
    int y{0};
};

Coordinate santaPosition;
```

Now that we have Santa's location, we have to store the path that Santa goes on.
The simplest way to describe this path is a **list** of `Coordinates`.

```c++
#include <vector>

using Path = std::vector<Coordinate>
Path santaPath;
```

We have finally every structures to handle the problem.
Now, we have to get the path taken by Santa.

```c++
// Adds the house where Santa start to deliver presets
santaPath.emplace_back(santaPosition);

for(auto direction : input)
{
    // Modify Santa's location
    switch (direction)
    {
        case '^': ++santaPosition.y; break;
        case '>': ++santaPosition.x; break;
        case '<': --santaPosition.x; break;
        case 'v': --santaPosition.y; break;
    }
    // Adds the new location to Santa's path
    santaPath.emplace_back(santaPosition);
}
```

Finally, we have the path taken by Santa.7
All we need to do now, is to found out the number different houses there is on Santa's path.
For that, we are going to use 2 std algorithms : [std::sort](https://en.cppreference.com/w/cpp/algorithm/sort) and [std::unique](https://en.cppreference.com/w/cpp/algorithm/unique).

```c++
#include <algorithm>

std::sort(std::begin(santaPath), std::end(santaPath));
auto it = std::unique(std::begin(santaPath), std::end(santaPath));
const auto numberOfHousesVisited = std::distance(std::begin(santaPath), it);
```

Note that, to be able to use those algorithms, we have to specify the operators `==` and `<` of the structure `Coordinate`.

I encourage you to go look at the full solution used in this sample of code on my [GitHub](https://github.com/Xav83/AdventOfCode/tree/2015.03/2015/Day3).

## Part 2

### Problem

This problem is the same as the part one, except that now, there are two "delivery men", Santa and Robo-Santa.
Both start at the same position and take turns moving based on instructions from the elf.
We still have to found out **how many houses receive at least one present**.

For example:
- `^v` delivers presents to `3` houses, because Santa goes north, and then Robo-Santa goes south.

### Solution

Most of the source code is very similar to the part 1, so we will only focus on the differences.
To start, we can integrate a common structure to Santa and Robo-Santa, that I called `DeliveryMan`.

```c++
struct DeliveryMan
{
    Coordinate position;
    Path path;
};

DeliveryMan santa, roboSanta;
```

When collecting the instruction, we can switch between the delivery man receiving the instruction with a ternary instruction.

```c++
DeliveryMan deliveryMan;

deliveryMan = deliveryMan == &santa ? &roboSanta: &santa;
```

And finally, once we've sorted both Santa and Robo-Santa paths, we have to **merge** them before using std::unique.

```c++
    std::vector<Coordinate> mergedPath;
    std::merge(std::begin(santa.path), std::end(santa.path), std::begin(roboSanta.path), std::end(roboSanta.path), std::back_inserter(mergedPath));
```

And that's it about the interesting point of this part. You can check the full solution of this code on my [GitHub](https://github.com/Xav83/AdventOfCode/tree/2015.03/2015/Day3).

## Conclusion

You can note that the solutions written in this post, don't include all the sources to make running programs, but only the interesting part of the sources to solve this problem.
If you want to see the programs from end to end, you can go on my [GitHub account](https://github.com/Xav83/AdventOfCode/tree/2015.03/2015/Day3), explore the full solution, add comments or ask questions if you want to.

Here is the list of std methods and containers that we have used, I can't encourage you enough to look at their definitions :

- [std::vector](https://en.cppreference.com/w/cpp/container/vector)
- [std::sort](https://en.cppreference.com/w/cpp/algorithm/sort)
- [std::unique](https://en.cppreference.com/w/cpp/algorithm/unique)
- [std::distance](https://en.cppreference.com/w/cpp/iterator/distance)
- [std::merge](https://en.cppreference.com/w/cpp/algorithm/merge)
- [std::back_inserter](https://en.cppreference.com/w/cpp/iterator/back_inserter)

Thanks for you reading, hope you liked it 😃

And until next part, have fun learning and growing.
