# Advent Of Code - Doesn't He Have Intern-Elves For This ? - Puzzle 5

Hello ! I'm Xavier Jouvenot and here is the third part of a long series on [Advent Of Code](https://adventofcode.com). You can find the previous part [here](2019-06-21-AdventOfCode2015-Day4-The-Ideal-Stocking-Stuffer.md)

For this new post, we are going to solve the problem from the 5th December 2015, named "Doesn't He Have Intern-Elves For This?".
The solution I will propose in C++, but the reasoning can be applied to other languages.

But first, I want to address a point on the previous problem, but if you want to jump on the Day 5 problem, [be my guest](#Part1).

## Back to Day 4

[TheFlamefire](https://www.reddit.com/user/TheFlamefire/), a Reddit user, gave me some good comments to help me improve the solution for the [Day 4 problem](2019-06-21-AdventOfCode2015-Day4-The-Ideal-Stocking-Stuffer.md). His comments resonates directly with the [Single Responsibility principle](https://en.wikipedia.org/wiki/Single_responsibility_principle), which I wanted to talk about in this Day 5 problem (nice coincidence 😄).

### Single Responsibility principle, what is that ?

As well described by [Wikipedia](https://en.wikipedia.org/wiki/Single_responsibility_principle):

> The single responsibility principle is a computer programming principle that states that every module, class, or function should have responsibility over a single part of the functionality provided by the software, ...

By applying this principle, you will end up with a much modular code with smaller and more understandable chunk of code for anyone to read and work on. There are many other great advantages to adopt this principle, but I won't describe them here. Moreover, this principle is the first part of the [SOLID principles](https://en.wikipedia.org/wiki/SOLID), which will deserved several blogs post to explain it correctly.

### Single Responsibility principle in the Day 4

In the Day 4 solution, we've created a function named `getMd5` which passes the private key to the MD5 algorithm **AND** convert the result from an array of `unsigned char` to a `std::string`. The "AND" that I've highlighted is the thing that is suspicious, since it indicates that the function does 2 things, which violates the Single Responsibility principle. To correct it I had to split the `getMd5` function into two functions:

- `getMd5` which only returns the array of byte corresponding to the MD5 hash generated from the private key.
- `bytesToString` which converts a array of `unsigned char` to a `std::string`

I can only encourage you to go take a look to the implementation of those function on my [Github](https://github.com/Xav83/AdventOfCode/tree/2015.05/2015/Day4) and now, let's go to the Day 5 ! 😈

## Part 1

### The Problem

The full version of this problem can be found directly on the [Advent of Code website](https://adventofcode.com/2015/day/5), I will only describe the essence of the problem here:

Santa needs help figuring out which strings in his text file are naughty or nice.
A nice string is one with all of the following properties:

- It contains at least three vowels
- It contains at least one letter that appears twice in a row
- It does not contain the strings `ab`, `cd`, `pq`, or `xy`

### Solution

As in the point on the day 4, for this problem, we are going to use the [Single Responsibility prinple](https://en.wikipedia.org/wiki/Single_responsibility_principle). Indeed, this problem is well adapt to introduce it, since each bullet on the list of nice string properties can be put in one function with one responsibility. So we end up with three functions :

- `bool hasThreeVowels(const std::string_view str)`
- `bool hasLettersAppearingTwiceInRow(const std::string_view str)`
- `bool hasForbiddenSubString (const std::string_view str)`

Let's dig deeper in those functions.

First for the `hasThreeVowels` function, we can directly use the `std::count_if` algorithm to get the right behavior like that : 

```c++
    return 3 <= std::count_if(std::begin(str), std::end(str), [](const auto& letter)
    {
        return letter == 'a' || letter == 'e' || letter == 'i' || letter == 'o' ||letter == 'u';
    });
```

To respect the Single Responsibility principle, we could event have created a function `isVowel` instead of directly include the condition in the lambda used here.

For the second function, `hasLettersAppearingTwiceInRow`, I didn't find a std algorithm allowing me to have a handy solution, so I use the naive solution using a for ranged-based loop :

```c++
char previousCharacter = CHAR_MIN;
for(const auto& character : str)
{
    if(character == previousCharacter) { return true; }
    previousCharacter = character;
}
return false;
```

And for the last function, `hasForbiddenSubString`, we can use the `std::string_view::find` method:
```
    return str.find("ab") != std::string_view::npos ||
           str.find("cd") != std::string_view::npos ||
           str.find("pq") != std::string_view::npos ||
           str.find("xy") != std::string_view::npos;
```

Now that we have those 3 functions, we have 2 thing left to do.
First of all, we have to create a function defining what a nice string is, by combining the previous functions in a condition :
```
bool isNice(const std::string_view str)
{
    return hasThreeVowels(str) &&
           hasLettersAppearingTwiceInRow(str) &&
           ! hasForbiddenSubString(str);
}
```

And apply this function on each line of the input:
```c++
auto numberOfNiceString{0};
foreachLineIn(fileContent, [&numberOfNiceString](const std::string& str)
{
    if(isNice(str))
    {
        ++numberOfNiceString;
    }
});
std::cout << numberOfNiceString;
```

And voilà, we have a pretty solution and it applies the Single Responsibility principle while finding Santa's nice strings. 👼

## Part 2

### The Problem

Actually, Santa made some mistake, and the criteria used to find the naughty and the nice strings were wrong. Indeed a nice string has the following properties:

- It contains a pair of any two letters that appears at least twice in the string without overlapping
- It contains at least one letter which repeats with exactly one letter between them

### Solution

Dammit, so you tell me we have to do it again ?! 😮

Well actually no. Since we applied the Single Responsibility principle, the logic of our program stays the same, we only have to adapt `isNice` method by using two new functions, one for each property of the nice strings :
```c++
bool isNice(const std::string_view str)
{
    return hasTwiceAPairOfLetters(str) &&     
           hasOneLetterAppearingTwiceWithOnlyOneLetterBetween(str);
}
```

Now, all we have to do, is to implements those functions.
First, let's take a look at `hasTwiceAPairOfLetters` implementation:

```c++
for(size_t i = 0; i < str.size() - 2; ++i)
{
    std::string_view str_toSearch(&str[i], 2); // The first 2 elements of the string
    std::string_view str_toSearchIn(&str[i + 2], str.size() - i - 2); // The rest of the string
    if(str_toSearchIn.find(str_toSearch) != std::string_view::npos)
    {
        return true;
    }
}
return false;
```

I don't have much to say about this implementation except that `std::string_view` does a pretty good job 😃

And now, the function `hasOneLetterAppearingTwiceWithOnlyOneLetterBetween`: 
```c++
for(size_t i = 0; i < str.size() - 2; ++i)
{
    if(str[i] == str[i + 2]) { return true; }
}
return false;
```

And finally, we have now a pretty definition for the nice strings which will satisfy Santa 🎅

## Conclusion

As always, you can note that the solutions written in this post, don't include all the sources to make running programs, but only the interesting part of the sources to solve this problem.
If you want to see the programs from end to end, you can go on my [GitHub account](https://github.com/Xav83/AdventOfCode/tree/2015.05/2015/Day5), explore the full solution, add comments or ask questions if you want to.

Here is the list of std methods and containers that we have used, I can't encourage you enough to look at their definitions :

- [std::string_view](https://en.cppreference.com/w/cpp/string/basic_string_view)
- [std::string_view::find](https://en.cppreference.com/w/cpp/string/basic_string_view/find)
- [std::string_view::npos](https://en.cppreference.com/w/cpp/string/basic_string_view/npos)
- [std::count_if](https://en.cppreference.com/w/cpp/algorithm/count)

Thanks for you reading, hope you liked it. For my part, I've fun doing it 😃

And until next part, have fun learning and growing.
