# Advent Of Code - Corporate Policy - Puzzle 11

Hello ! I'm Xavier Jouvenot and here is the eleventh part of a long series on [Advent Of Code](https://adventofcode.com). You can find the previous part [here](2019-09-09-AdventOfCode2015-Elves-Look-Elves-Say.md)

For this new post, we are going to solve the problem from the 11th December 2015, named "Corporate Policy".
The solution I will propose in C++, but the reasoning can be applied to other languages.

## Part 1

### The Problem

The full version of this problem can be found directly on the [Advent of Code website](https://adventofcode.com/2015/day/11), I will only describe the essence of the problem here:

There is a new policy on the password in the North Pole, and Santa's password just expired. To remember his new password, Santa increments his old password string repeatedly until it is valid. To increment is just like to count with numbers: `xx`, `xy`, `xz`, `ya`, `yb`, and so on. Increase the rightmost letter one step; if it was z, it wraps around to a, and repeat with the next letter to the left until one doesn't wrap around.

The policy specifies that the password must match the following requirements:
Unfortunately for Santa, a new Security-Elf recently started, and he has imposed some additional password requirements:
- Passwords must have eight letters
- Passwords must include one increasing straight of at least three letters, like `abc`.
- Passwords may not contain the letters `i`, `o`, or `l`, which are confusing letters.
- Passwords must contain at least two different, non-overlapping pairs of letters, like `aa`.

For example, the next password after `abcdefgh` is `abcdffaa`.
And now, we have to find Santa's new password.

### Solution

First of all, we have to be able to "increment" a password.
Indeed, a character doesn't have natively such behavior, so we must create how own `PasswordCharacter`:

```c++
class PasswordChar
{
public:
    // ... others functions

    void increment()
    {
        value = willWrapAroundToA () ? 'a' : value + 1;
    }

    bool willWrapAroundToA () const { return value == 'z'; }

private:
    char value{'a'};
};
```

Here you have only the methods that will be interesting for this exercise.
So we have a `PasswordChar` that can be increment, and when it reach the letter 'z', it will wrap around, and we will be able to get that information to be able to increment the next letter is necessary.

Now that we have a character that can be incremented, we need now a password composed of several of those character and which can be incremented too.

```c++
class Password
{
    // ... other functions

    void increment ()
    {
        for(int index {static_cast<int>(value.size()) - 1}; index >= 0 ; --index)
        {
            const auto characterWillWrap = value[index].willWrapAroundToA();
            value[index].increment();
            if(!characterWillWrap)
            {
                break;
            }
        }
    }

private:
    std::array<PasswordChar, 8> value;
};
```

So there he is, the password !
To make sure is matches the first requirement, we will use an `std::array` of size `8`.
And we have a increment function too, which, when called, is going to increment each letter of the password, from the end, to the beginning, but stop incrementing is one character does wrap around after an increment.

Good, now we have a password that can be incremented, and that matches the first requirement.
All we have to do then, is to make sure that the password is safe, which mean that it match the three last requirements and we are done. Following the Single Responsibility Principle, we will have three functions (actually, some methods since I've made them part of the Password class).

Let's start with the simplest, the forbidden letters.

```c++
bool Password::hasNoConfusingLetters() const
{
    return std::none_of(std::begin(value), std::end(value), [](const auto& passwordChar)
    {
        return passwordChar == 'i' || passwordChar == 'o' || passwordChar == 'l';
    });
}
```

The std makes it so simpler, don't you think ? This std function, `none_of`, will return `true`of *none of* the element in `value` return `true` when passed to the lambda function. So useful ! I love it 😍

Well, now, lets see the next requirement, the increasing straight of three letters

```c++
bool Password::hasIncreasingStraight() const
{
    for(auto index = size_t{0}; index < value.size() - 2; ++index)
    {
        if(value[index] == value[index + 1] - 1 && value[index] == value[index + 2] - 2)
        {
            return true;
        }
    }
    return false;
}
```

No std algorithm for this one, sadly, but still pretty straight forward.
We have to be careful with the bounds, to make sure we stay in the array of characters, not like I did when I've first written this function 😝

And finally the last requirement, the two non-overlapping pairs:

```c++
bool Password::hasTwoNonOverlappingPairs() const
{
    bool hasAtLeastOnePair{false};
    for(auto index = size_t{0}; index < value.size() - 1; ++index)
    {
        if(value[index] == value[index + 1])
        {
            if(hasAtLeastOnePair)
            {
                return true;
            }
            hasAtLeastOnePair = true;
            ++index;
        }
    }
    return false;
}
```

This one is far less easy to read. So I'm going to explain it a little more in detail.

We are go through the password.
We check if there is a pair between the current letter and the next one (so we stop just before the last letter to avoid to be out of the array).
If this isn't a pair, we go to the next letter, but is it is a pair, that is when it gets interesting.
We check we already have found a pair.
If this is a case, that it, we match the requirement. But if not, that mean that is the first pair we have found.
So we register this information, and skip the next letter to avoid to found overlapping pairs.

Now, we can combine, with && operators, those functions in one, named `isSafe` to easily know if a password is safe.

So now we can increment a password and check is a password matches the requirements.
All we have to do, is to combine those features to found the next safe password for Santa

```c++
Password newPassword (oldPassword);
do
{
    newPassword.increment();
} while (!newPassword.isSafe());
```

Voilà ! By the end of this loop, we have our password ready for Santa to use it 😄🎆

## Part 2

We will go quickly on this part, since it the problem to solve is the same.
Indeed, in this part, we need to find the next password of the password we have found in the previous part.

Personally, I've directly passed the password of the part 1 to the same program, to found the solution of this part.
But if you want, you can also modify the program to make it found the second next password, by running the `do-while` loop twice.

I encourage you to look at the solution on my [GitHub](https://github.com/Xav83/AdventOfCode/tree/2015.11/2015/Day11) to look how I did that, since I use [CMake](https://cmake.org/) to build and check the solution I've found.

## Conclusion

You can note that the solutions, written in this post, don't include all the sources to make running programs, but only the interesting part of the sources to solve this problem.
If you want to see the programs from end to end, you can go on my [GitHub account](2019-09-09-AdventOfCode2015-Day10-Elves-Look-Elves-Say.md), explore the full solution, add comments or ask questions if you want to, on the platform you read this article, it will also help me improve the quality of my articles.

Here is the list of std method that we have used, I can't encourage you enough to look at their definitions :

- [std::none_of](https://en.cppreference.com/w/cpp/algorithm/all_any_none_of)
- [std::array](https://en.cppreference.com/w/cpp/container/array)
- [std::begin](https://en.cppreference.com/w/cpp/iterator/begin)
- [std::end](https://en.cppreference.com/w/cpp/iterator/end)

Thanks for you reading, hope you liked it 😃

And until next part, have fun learning and growing.
