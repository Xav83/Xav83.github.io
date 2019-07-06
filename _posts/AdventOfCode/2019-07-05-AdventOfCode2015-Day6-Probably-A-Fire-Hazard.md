# Advent Of Code - Probably a Fire Hazard - Puzzle 6

Hello ! I'm Xavier Jouvenot and here is the sixth part of a long series on [Advent Of Code](https://adventofcode.com). You can find the previous part [here](2019-06-28-AdventOfCode2015-Day5-Doesn-t-He-Have-Intern-Elves-For-This.md)

For this new post, we are going to solve the problem from the 6th December 2015, named "Probably a Fire Hazard".
The solution I will propose in C++, but the reasoning can be applied to other languages.

## Part 1

### The Problem

The full version of this problem can be found directly on the [Advent of Code website](https://adventofcode.com/2015/day/6), I will only describe the essence of the problem here:

You want to defeat your neighbors in the holiday house decorating contest.
For that you deploy a 1000x1000 grid of light and follows Santa instruction to display the ideal lighting configuration.

There are 3 kind of instructions:

- **turn on** `0,0` through `999,999` would turn on (or leave on) every light.
- **toggle** `0,0` through `999,0` would toggle the first line of 1000 lights, turning off the ones that were on, and turning on the ones that were off.
- **turn off** `499,499` through `500,500` would turn off (or leave off) the middle four lights.

### Solution

Here is the main part of the algorithm solving this problem :

```c++
    foreachLineIn(fileContent, [&grid](const std::string& line)
    {
        const auto actionId = getActionFromLine(line);
        const auto rectangle = getRectangleFromLine(line);
        switch (actionId)
        {
        case LightAction::TURN_ON:
            grid.turnOn(rectangle);
            break;
        case LightAction::TURN_OFF:
            grid.turnOff(rectangle);
            break;
        case LightAction::TOGGLE:
            grid.toggle(rectangle);
            break;
        default:
            assert(false);
            break;
        }
    });

     std::cout << grid.getNumberOfLightTunedOn();
```

In plain English, this code mean that, for each in instructions we identify the kind of instructions, the are concerned on the grid and apply the instruction.

The rest of the code is pretty straight forward and can be find on my [GitHub](https://github.com/Xav83/AdventOfCode/tree/2015.06/2015/Day6).

As a summary, I can tell you that there are five more elements:

```c++
enum class LightAction; // Qualifies which kind of instruction we are dealing with
class Coordinate { int x, y; } // Object representing a position on a grid (use in Day 3)
class Rectangle { Coordinate topLeft, bottomRight; } // Object representing an area
class Light { bool isOn{false}; }; // Object representing one light of the grid
class Grid { std::array<std::array<Light>> lights; } ; // Object representing the grid of lights
```

## Part 2

### The Problem

Actually, you mistranslated the instructions that Santa gave you (version original in Ancient Nordic Elvish).
The real meaning of the instructions are:

- **turn on** means you should increase the brightness of those lights by `1`.
- **toggle** means you should increase the brightness of those lights by `2`.
- **turn off** means you should decrease the brightness of those lights by `1`.

### Solution

Well, for this part, the structure of the code is basically the same, only the implementation details and the naming of some functions (from `getNumberOfLightTurnedOn` to `getBrightness`) have changed, so I won't go on in details, you can find the solution of this part on my [GitHub](https://github.com/Xav83/AdventOfCode/tree/2015.06).

```c++
// Part One
class Light { bool isOn{false}; };
// Part Two
class Light { size_t brightness{0}; };
```

Actually, I have encountered an interesting problem that I didn't expect.
Indeed, I'm developing on Windows for this Advent of Code, and I face a problem during the compilation. I was having weird error messages only by adding an integer in the `Light` class (to represent the brightness).

After some digging, I found that it came from the stack size, which was too little by default. To fix the problem, I added the link option [/STACK](https://docs.microsoft.com/fr-fr/cpp/build/reference/stack-stack-allocations?view=vs-2019) and set it to 10 MO (instead of 1 by default), and everything compiled again (youhou \o/).

## Conclusion

As always, you can note that the solutions written in this post, don't include all the sources to make running programs, but only the interesting part of the sources to solve this problem.
If you want to see the programs from end to end, you can go on my [GitHub account](https://github.com/Xav83/AdventOfCode/tree/2015.0-6/2015/Day6), explore the full solution, add comments or ask questions if you want to.

Thanks for you reading, hope you liked it. For my part, I've fun doing it 😃

And until next part, have fun learning and growing.
