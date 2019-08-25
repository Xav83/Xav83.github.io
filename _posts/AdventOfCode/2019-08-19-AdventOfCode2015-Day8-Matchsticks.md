# Advent Of Code - Matchsticks - Puzzle 8

Hello ! I'm Xavier Jouvenot and here is the eighth part of a long series on [Advent Of Code](https://adventofcode.com). You can find the previous part [here](2019-08-12-AdventOfCode2015-Day7-Some-Assembly-Required.md)

For this new post, we are going to solve the problem from the 8th December 2015, named "Matchsticks".
The solution I will propose in C++, but the reasoning can be applied to other languages.

## Part 1

### The Problem

The full version of this problem can be found directly on the [Advent of Code website](https://adventofcode.com/2015/day/8), I will only describe the essence of the problem here:

This year, Santa is bringing his list as a digital copy.
He needs us to find out how much space it will take up when stored.
To do that, we will have to find the difference between the *number of characters in the code representation* of the string literal and the *number of characters in the in-memory string* itself.

For exemple:

- "aaa\"aaa\x27" is 14 characters of code, but the string itself contains six "a" characters, a single quote character (escaped with the "\"), and an `'` in hexadecimal, so 8 characters in memory, which means the result of the difference will be `14-8=6`.

For information, the only escape sequences used are `\\` (representing a single backslash), `\"` (representing a lone double-quote character), and `\x` plus two hexadecimal characters (representing a single character with that ASCII code)

### Solution

Compared to the previous problem, this one is simpler, so the solution will be much quicker.
Let's start with the logic of the program !

```c++
size_t totalNumberOfCharacter{0};
size_t inMemoryNumberOfCharacter{0};

foreachLineIn(fileContent, [&totalNumberOfCharacter, &encodedNumberOfCharacter](const std::string& line)
{
    totalNumberOfCharacter += getTotalNumberOfCharacters(line);
    inMemoryNumberOfCharacter += getNumberOfCharacterInMemory(line);
});

const auto result =  totalNumberOfCharacter - inMemoryNumberOfCharacter;
```

Easy right ?! Well the objective is to make a subtraction, so, for each line, we do the get the data and make the subtraction at the end.

Now, all we need to do is to look at the detail of the functions `getTotalNumberOfCharacters`  and `getNumberOfCharacterInMemory`:

The first one (`getTotalNumberOfCharacters`) is a one-liner, all we have to do is return `line.size()`.

The second one is larger, but still pretty simple.
Indeed, to find the number of characters when the string is encoded, we have to count each letter, and count only one for the escape sequences of characters. This look like that:
```c++
size_t size{0};
for(size_t index{0}; index < line.size(); ++index)
{
    ++size;

    if(index == line.size() - 1) { continue; }

    if(line[index] == '\\' && line[index + 1] == '\\') { ++index; }
    else if(line[index] == '\\' && line[index + 1] == '"') { ++index; }
    else if(line[index] == '\\' && line[index + 1] == 'x') { index += 3; }
}
return size - 2; // Subtracts the two " around the sequence of characters
```

And here we are, with the source code allowing us to give Santa his answer 🎅

## Part 2

### The Problem

Now, let's do the opposite.
In addition to finding the ù, we are going to encode each code representation as a new string and find the *number of characters of the new encoded representation*, including the surrounding double quotes, before subtracting it to the *number of characters of code*.

For exemple:

- "aaa\"aaa\x27" encodes "\"aaa\\\"aaa\\x27\"", which adds `7` characters.

### Solution

Well, since this part is very similar to the first part, we can reuse the code and only replace the call to `getNumberOfCharacterInMemory` by a call to a function `getNumberOfCharactersWhenEncoded`.

So let's take a look to this new function:

```c++
std::stringstream ss;
ss << std::quoted(line);
return ss.str().size();
```

What really ?!... Yes really, C++ has something done exactly for that, [std::quoted](https://en.cppreference.com/w/cpp/io/manip/quoted), to encode a sequence of characters.

And we are done with this day from Advent of Code.
It's pretty nice to have some simpler problem for time to time 😃

## Conclusion

You can note that the solutions, written in this post, don't include all the sources to make running programs, but only the interesting part of the sources to solve this problem.
If you want to see the programs from end to end, you can go on my [GitHub account](https://github.com/Xav83/AdventOfCode/tree/2015.08/2015/Day8), explore the full solution, add comments or ask questions if you want to.

Here is the list of std method that we have used, I can't encourage you enough to look at their definitions :

- [std::quoted](https://en.cppreference.com/w/cpp/io/manip/quoted)

Thanks for you reading, hope you liked it 😃
And until next part, have fun learning and growing.
