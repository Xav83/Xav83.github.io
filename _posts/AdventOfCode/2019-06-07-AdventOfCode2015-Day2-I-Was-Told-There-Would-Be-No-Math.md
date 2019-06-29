# Advent Of Code - I Was Told There Would Be No Math - Puzzle 2

Hello ! I'm Xavier Jouvenot and here is the second part of a long series on [Advent Of Code](https://adventofcode.com). You can find the previous part [here](2019-06-01-AdventOfCode2015-Day1-NotQuiteLisp.md)

For this new post, we are going to solve the problem from the 2nd December 2015, named "I Was Told There Would Be No Math".
The solution I will propose in C++, but the reasoning can be applied to other languages.

## Part 1

### The Problem

The full version of this problem can be found directly on the [Advent of Code website](https://adventofcode.com/2015/day/2), I will only describe the essence of the problem here:

Santa's elves need to order more wrapping papers for the presents.
To do so, we know we need, for each present to have enough wrapping paper to cover the surface area of the present's box plus some extra wrapping paper, about the smallest side of the box of the present.
For reminder, the surface area of the present's box is `2*l*w + 2*w*h + 2*h*l`, with `l` the length, `w` the width and `h` the height of the box.

For example:
- A present with dimensions `2x3x4` requires `2*6 + 2*12 + 2*8 = 52` square feet of wrapping paper plus `6` square feet of slack, for a total of `58` square feet.

### Solution

```c++
#include <algorithm>

auto wrappingPaperArea{0};
foreachLineIn(fileContent, [&wrappingPaperArea](const auto& line)
{
    const auto dimension = makeDimensionFromLine(line);
    const auto areas = dimension.toAreas();
    const auto min = std::min_element(std::begin(areas), std::end(areas));
    wrappingPaperArea += 2 * areas[0] + 2 * areas[1] + 2 * areas[2] + *min;
});
```

This source code shows the main part of the solution. There are 3 parts in this solution.

First, we have to get the information from the input. For that, we use the method `foreachLineIn` and `makeDimensionFromLine` we have created and the structure `Dimension` to store the data. `foreachLineIn` allows us to treat each line of the input, each present, at a time. `makeDimensionFromLine` extracts from the text describing the present's dimension into usable data.

Then, we can calculate the areas with the dimensions and know which area is the smallest with `Dimension::toAreas()` and `std::min_element`.

And finally, we calculate the amount of wrapping paper we need for the present.

I encourage you to go look at the implementation of the methods used in this sample of code on my [Github](https://github.com/Xav83/AdventOfCode/tree/master/2015/Day2).

And, without a surprise this time, the **Part 2** of the problem

## Part 2

### Problem

Now, the Santa's elves are running low on the ribbon, and since a ribbon is all the same width, we only have to find the length they need to order. To determine this length, here are the information that we have:
- the ribbon required to wrap a present is the shortest distance around its sides.
- Santa's elves also need to make a bow with the ribbon, which is about the cubic feet of volume of the present.

With the same present with dimensions `2x3x4` from the **Part 2**, we have to order `2+2+3+3 = 10` feet of ribbon to wrap the present plus `2*3*4 = 24` feet of ribbon for the bow, for a total of `34` feet.

### Solution

```c++
auto ribbonLength{0};
foreachLineIn(fileContent, [&ribbonLength](const auto& line)
{
    const auto dimension = makeDimensionFromLine(line);
    const auto max = std::max({dimension.length, dimension.width, dimension.height});
    ribbonLength += 2 * dimension.length + 2 * dimension.width + 2 * dimension.height - 2 * max + dimension.length * dimension.width * dimension.height;
});
```

This source code also shows the main part of the solution.

As in the previous part, we get the dimension of a present. Then, instead of finding the two smallest of the dimension of the present to calculate the shortest distance around its sides, we can subtract the max from the calculation. Doing so, we obtain the calculation from above for the ribbon of one present.

## Conclusion

You can note that the solutions written in this post, don't include all the sources to make running programs, but only the interesting part of the sources to solve this problem.
If you want to see the programs from end to end, you can go on my [GitHub account](https://github.com/Xav83/AdventOfCode/tree/master/2015/Day2), explore the full solution, add comments or ask questions if you want to.

Here is the list of std method that we have used, I can't encourage you enough to look at their definitions :

- [std::min_element](https://en.cppreference.com/w/cpp/algorithm/min_element)
- [std::max](https://en.cppreference.com/w/cpp/algorithm/max)

Thanks for you reading, hope you liked it 😃
And until next part, have fun learning and growing.
