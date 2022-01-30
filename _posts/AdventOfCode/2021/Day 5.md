# Advent Of Code 2021 - Hydrothermal Venture - Puzzle 5

Hello ! I'm Xavier Jouvenot and here is the 5th part of a long series on [Advent Of Code 2021](https://adventofcode.com).

For this new post, we are going to solve the problem from the 5th December 2021, named "Hydrothermal Venture".
The solution I will propose in C++, but the reasoning can be applied to other languages.

_Self promotion_: You can find other articles on computer science and programming on my [website](www.10xlearner.com) 😉

## Part 1

### The Problem

The full version of this problem can be found directly on the [Advent of Code website](https://adventofcode.com/2021/day/5), I will only describe the essence of the problem here:

After playing bingo with a Giant Squid, during the Day 4, we are near the ocean floor and facing a field of hydrothermal vents, which produce big clouds that we should avoid. To help us, the submarine produces a list of the nearby lines of vents:
```
0,9 -> 5,9
8,0 -> 0,8
9,4 -> 3,4
2,2 -> 2,1
7,0 -> 7,4
6,4 -> 2,0
0,9 -> 2,9
3,4 -> 1,4
0,0 -> 8,8
5,5 -> 8,2
```
Each line is represented by the coordinate of the start of the line and the coordinate of the end of the line.
For now, we are only considering the horizontal lines and the vertical lines, and determine the number of dangerous areas, which are the points where at least 2 lines of vents are traversing.

### Solution

As you are going to see, the solution of this day's problem is pretty simple.
So this is a good time to go into the detail of the `std` functions used and why they are used.

First of all, I adapted the input to avoid having to parse a text file, so the input look like that:
```cpp
struct Point
{
    int x{0}, y{0};
};

struct Line
{
    Point start{0, 0}, end{0, 0};
};

constexpr std::array<Line, 500> input_lines{
    Line {{973,543}, {601,915}},
    Line {{758,846}, {758,168}}, 
    /* ... */
    Line {{231,394}, {231,811}}
};
```

Here, we have 2 structures to simplify the manipulation of the data : the `Point` and the `Line` structure. The first one allows us to easily store the point coordinates while the second one is constituted from 2 points. Finally, we create an array of Lines, which is our puzzle input using the container [std::array](https://en.cppreference.com/w/cpp/container/array) which is, using the keyword [constexpr](https://en.cppreference.com/w/cpp/language/constexpr), evaluated at compile time.

Then, we create some utilitarian functions that we are going to need to solve the problem at hand.
We will need 3 functions:
```cpp
constexpr bool isVertical(const Line& line) { return line.start.y == line.end.y; }
constexpr bool isHorizontal(const Line& line) { return line.start.x == line.end.x; }
constexpr int getGridSize() {
    int max{0};
    for(const auto& line : input_lines)
    {
        max = std::max({max, line.start.x, line.start.y, line.end.x, line.end.y});
    }
    return max;
}
```

The 2 first functions allow us to know when a line is vertical (meaning that the Y axis doesn't change) or horizontal (meaning the X axis doesn't change).
The last one allow us to the size of the plan we are looking at. Indeed, to find the solution of the problem, we need to "draw" the lines and look to the place where the lines cross, and to do that, we need to have a space big enough to "draw" the lines. This size is obtained by finding the biggest coordinate in the list of Lines by using [std::max](https://en.cppreference.com/w/cpp/algorithm/max).

So now, we have everything ready to solve the problem !
We are going to do it in 2 steps: Drawing the lines **and** counting the number of dangerous areas.
```cpp
std::array<std::array<int, getGridSize()>, getGridSize()> grid{0};
for(const auto& line : input_lines) {
    if(isVertical(line)) {
        auto[min, max] = std::minmax(line.start.x, line.end.x);
        for(int i = min; i<= max; ++i) {
            ++grid[i][line.start.y];
        }
    }
    if(isHorizontal(line)) {
        auto[min, max] = std::minmax(line.start.y, line.end.y);
        for(int i = min; i<= max; ++i) {
            ++grid[line.start.x][i];
        }
    }
}
```
So we draw the lines in a grid we created to the right dimension (so that no line outside of the grid).
The drawing principle is pretty simple: when placing a line in the grid, we increment the value the corresponding points in the grid by 1. This means that the value of the Point in the grid represents the number of line going through that point.

In this sample of code, I want to focus a little bit on the use of [std::minmax](https://en.cppreference.com/w/cpp/algorithm/minmax) and the [structured binding](https://en.cppreference.com/w/cpp/language/structured_binding) declaration in the line of code `auto[min, max] = std::minmax(line.start.y, line.end.y)`. Those 2 elements allows us, in one line of code to: find the minimum and the maximum value between 2 existing variables, and to create 2 new variables set to the minimum value and the maximum value.

*Note*: [std::minmax](https://en.cppreference.com/w/cpp/algorithm/minmax) also works with an array of values. 

Finally, at the end of the drawing, each point in the grid will give us the information that we need, and counting them will give us the solution we are looking for:
```cpp
const auto numberOfDangerousAreas = std::accumulate(std::begin(grid), std::end(grid), 0, [](const auto& sum, const auto& line){
    return sum + std::count_if(std::begin(line), std::end(line), [](const auto& point){ return point >= 2; }); 
});
std::cout << "The solution is: " << numberOfDangerousAreas << std::endl;
```

We could have use 2 for loop and a "counter" variable to obtain the same result, but here, we heavily used the `std` functions available in order to be more expressive, more English-like, about what it happening to external readers.
So, to calculate the number of dangerous areas, we `accumulate` the number of of dangerous area found on each line, and for the number of dangerous areas on each line, we `count` the point `if` its value is over `2`. So [std::accumulate](https://en.cppreference.com/w/cpp/algorithm/accumulate) and [std::count_if](https://en.cppreference.com/w/cpp/algorithm/count) are totally appropriate to determinate the result we are looking for.

<details>
  <summary>[Spoiler] Problem Answer</summary>

The puzzle answer was 5608.
</details>

## Part 2

### Problem

For the second part of the problem, today, we are only going to add the diagonal lines to the evaluation of the dangerous areas. But not all the diagonal line, only the one which are 45° (which makes the problem easier).

### Solution

The solution for this part is almost the same than for the previous part.
We only need to one function and a if statement when drawing the lines in the grid.

```cpp
constexpr bool isDiagonal(const Line& line) {
    return std::abs(line.start.x - line.end.x) == std::abs(line.start.y - line.end.y);
}
```
There isn't much to say about this function. The characteristic of diagonal lines which are 45° is that the difference between the start and the end of the line on the X axis is the same than the one of the Y axis.

Then, in the drawing loop, we draw the diagonal lines: 
```cpp
if(isDiagonal(line))
{
    auto lineSize = std::abs(line.start.x - line.end.x);
    auto xdirection = (line.start.x - line.end.x) < 0 ? 1 : -1 ;
    auto ydirection = (line.start.y - line.end.y) < 0 ? 1 : -1 ;
    for(int i = 0; i<= lineSize; ++i)
    {
        ++grid[line.start.x + (i * xdirection)][line.start.y + (i * ydirection)];
    }
}
```

In this small code, we determine the direction of the line, in order to draw the line from the starting point to end of the line. Not much to say about that, but with this modification, then the code which determine the number of dangerous areas now uses the grid draw with the diagonal line, giving us the answer to this part of the problem 😉

<details>
  <summary>[Spoiler] Problem Answer</summary>

The puzzle answer was 20299.
</details>

## Conclusion

You can note that the solutions written in this post, don't include all the sources to make running programs, but only the interesting part of the sources to solve this problem.
If you want to see the programs from end to end, you can go on my [GitHub account](https://github.com/Xav83/adventofcode2021/tree/main/Day%205), explore the full solution, add comments or ask questions if you want to, on the platform you read this article, it will also help me improve the quality of my articles.

--------------

Thank you all for reading this article,
And until my next article, have a splendid day 😉

![Advent Of Code 2021](https://raw.githubusercontent.com/Xav83/Xav83.github.io/master/res/Advent%20Of%20Code/2021/Screenshot%20Day%205.png)

## Interesting links

- [constexpr documentation](https://en.cppreference.com/w/cpp/language/constexpr)
- [std::array documentation](https://en.cppreference.com/w/cpp/container/array)
- [std::max documentation](https://en.cppreference.com/w/cpp/algorithm/max)
- [std::minmax documentation](https://en.cppreference.com/w/cpp/algorithm/minmax)
- [structured binding documentation](https://en.cppreference.com/w/cpp/language/structured_binding)
- [constexpr documentation](https://en.cppreference.com/w/cpp/language/constexpr)
- [std::accumulate documentation](https://en.cppreference.com/w/cpp/algorithm/accumulate)
- [std::count_if documentation](https://en.cppreference.com/w/cpp/algorithm/count)
- [std::abs documentation](https://en.cppreference.com/w/cpp/numeric/math/abs)
- [lambda](https://en.cppreference.com/w/cpp/language/lambda)
- [range-based for loop documentation](https://en.cppreference.com/w/cpp/language/range-for)
- [Advent of Code - Day 5](https://adventofcode.com/2021/day/5)
- [10xlearner website](www.10xlearner.com)
