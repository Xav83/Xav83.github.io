# Advent Of Code - Elves Look, Elves Say - Puzzle 10

Hello ! I'm Xavier Jouvenot and here is the tenth part of a long series on [Advent Of Code](https://adventofcode.com). You can find the previous part [here](2019-08-26-AdventOfCode2015-Day9-All-In-A-Single-Night.md)

For this new post, we are going to solve the problem from the 10th December 2015, named "Elves Look, Elves Say".
The solution I will propose in C++, but the reasoning can be applied to other languages.

## Part 1

### The Problem

The full version of this problem can be found directly on the [Advent of Code website](https://adventofcode.com/2015/day/10), I will only describe the essence of the problem here:

Today, the Elves are playing a game called `look-and-say` and we will play with them.
But first what is the `look-and-say` game about ? Well it's sequences that are generated iteratively, using the previous value as input for the next step, and to generate the next input, you describe what the input is. The example will make it all clear:

- `1` becomes `11` (1 copy of digit `1`).
- `11` becomes `21` (`2` copies of digit `1`).
- `21` becomes `1211` (one `2` followed by one `1`).
- `1211` becomes `111221` (one `1`, one `2`, and two `1`s).
- `111221` becomes `312211` (three `1`s, two `2`s, and one `1`).

So for this problem, we will have to process the `look-and-say` steps 40 times on the input given.

### Solution

Let's play then ! 😄

Obviously, following the Single Responsibility Principle, we are going to have a function `lookAndSay` which is going to do one iteration of the game. But first, let's get on the iteration process, which is simpler to explain and understand, it looks something like that:

```c++
constexpr auto numberOfProcessToRun = 40;
for(auto i = 0; i < numberOfProcessToRun ; ++i)
{
    input = lookAndSay(input); // with input a std::string either specified in the program or elsewhere
}
const auto result = input.size();
```

Pretty short, don't you think. Well it doesn't need to be longer.
Indeed, we iterate the 40 times and each time we set in the input variable the result of the `lookAndSay` function run with the current input, so it will be ready for the next iteration.
And in the end, we get the size, since it is the information that we want.

Now let's look and the core of the problem, the `lookAndSay` function:

```c++
std::string lookAndSay(const std::string_view input)
{
    std::string result;
    size_t index = 0;
    while (index < input.size())
    {
        const auto firstDistinctElement = std::find_if(std::begin(input) + index, std::end(input),[&input, &index](const auto& character){
            return character != input[index];
        });
        const auto count = std::distance(std::begin(input) + index, firstDistinctElement);
        result += std::to_string(static_cast<size_t>(count)) + input[index];
        index += count;
    }
    return result;
}
```

A bit more complex ^^
So what are we actually doing here, I'll explain it to you!
We iterate through the input, we take the character at the current `index`, and look for the first different characters in the following of the input.
Once we found it, we `count` the distance between the character at the current `index` and the characters that differs, which gives us the number of same characters.
Then, we store that in the result variable with the character, go directly to the different character and repeat the process, until we are at the end of the input.

And voilà, we have it, we can play the look-and-say game 😃

## Part 2

This part is going to be very short !
Indeed, all we have to do, it exactly the same except we have to apply the process 50 times.
So all we have to do, is to change the value of the `numberOfProcessToRun` from `40` to `50`, to do it. You can also make `numberOfProcessToRun` be an argument of your program, so you can use the same binary for both part, as I did on my [GitHub](https://github.com/Xav83/AdventOfCode/tree/2015.10/2015/Day10).

## Conclusion

You can note that the solutions, written in this post, don't include all the sources to make running programs, but only the interesting part of the sources to solve this problem.
If you want to see the programs from end to end, you can go on my [GitHub account](https://github.com/Xav83/AdventOfCode/tree/2015.10/2015/Day10), explore the full solution, add comments or ask questions if you want to, on the platform you read this article, it will also help me improve the quality of my articles.

Here is the list of std method that we have used, I can't encourage you enough to look at their definitions :

- [std::string_view](https://en.cppreference.com/w/cpp/string/basic_string_view)
- [std::string](https://en.cppreference.com/w/cpp/string/basic_string)
- [std::find_if](https://en.cppreference.com/w/cpp/algorithm/find)
- [std::distance](https://en.cppreference.com/w/cpp/iterator/distance)
- [std::begin](https://en.cppreference.com/w/cpp/iterator/begin)
- [std::end](https://en.cppreference.com/w/cpp/iterator/end)
- [std::to_string](https://en.cppreference.com/w/cpp/string/basic_string/to_string)

Thanks for you reading, hope you liked it 😃

And until next part, have fun learning and growing.
