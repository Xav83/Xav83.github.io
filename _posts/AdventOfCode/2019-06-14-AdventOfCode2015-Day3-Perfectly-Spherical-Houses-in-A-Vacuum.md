# Advent Of Code - Perfectly Spherical Houses in a Vacuum - Puzzle 3

Hello ! I'm Xavier Jouvenot and here is the third part of a long series on [Advent Of Code](https://adventofcode.com). You can find the previous part [here](2019-06-07-AdventOfCode2015-Day2-I-Was-Told-There-Would-Be-No-Math.md)

For this new post, we are going to solve the second problem from the 3rd December 2015, named "Perfectly Spherical Houses in a Vacuum".
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
The simpliest way to describe this path is a **list** of `Coordinates`.

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

I encourage you to go look at the full solution used in this sample of code on my [Github](https://github.com/Xav83/AdventOfCode/tree/master/2015/Day3).
