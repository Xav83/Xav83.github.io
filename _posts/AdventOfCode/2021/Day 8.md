# Advent Of Code 2021 - Seven Segment Search - Puzzle 8

Hello ! I'm Xavier Jouvenot and here is the 8th part of a long series on [Advent Of Code 2021](https://adventofcode.com).

For this new post, we are going to solve the problem from the 8th December 2021, named "Seven Segment Search".
The solution I will propose in C++, but the reasoning can be applied to other languages.

_Self promotion_:
Here are a few social networks where you can follow me and check my work as a programmer and a writer 😉 
[personal website](www.10xlearner.com), [Twitter](https://twitter.com/10xLearner), [Dev.to](https://dev.to/10xlearner), [CodeNewbie](https://community.codenewbie.org/xav83), [Medium](https://medium.com/@xavier-jouvenot), [GitHub](https://github.com/Xav83/)

## Part 1

### The Problem

The full version of this problem can be found directly on the [Advent of Code website](https://adventofcode.com/2021/day/8), I will only describe the essence of the problem here:

After escaping the giant whale during Day 7, we entered a cave system and we notice that the four-digit seven-segment displays in your submarine are malfunctioning.
Each digit of a seven-segment display is rendered by turning on or off any of seven segments named a through g:
```
  0:      1:      2:      3:      4:
 aaaa    ....    aaaa    aaaa    ....
b    c  .    c  .    c  .    c  b    c
b    c  .    c  .    c  .    c  b    c
 ....    ....    dddd    dddd    dddd
e    f  .    f  e    .  .    f  .    f
e    f  .    f  e    .  .    f  .    f
 gggg    ....    gggg    gggg    ....

  5:      6:      7:      8:      9:
 aaaa    aaaa    aaaa    aaaa    aaaa
b    .  b    .  .    c  b    c  b    c
b    .  b    .  .    c  b    c  b    c
 dddd    dddd    ....    dddd    dddd
.    f  e    f  .    f  e    f  .    f
.    f  e    f  .    f  e    f  .    f
 gggg    gggg    ....    gggg    gggg
```

The problem is that the signals controlling the segments are now mixed up on each display. Indeed, the wires connected to the segments are now connected randomly one each four-digit display. So we need for each display to be able to recognize what new pattern reprensents which number. To help us, the submarine give us the 10 different patterns possible on each display and then an output value that should be on this display, something like:
```
acedgfb cdfbe gcdfa fbcad dab cefabd cdfgeb eafb cagedb ab | cdfeb fcadb cdfeb cdbaf  
```

To help us, the problem recognizes that the numbers `1`, `4`, `7` and `8`, have a unique number of segments, meaning that even with the wires all mixed up, if the pattern has the number of segment of those members, then it represents this number. So for this first part, we are going to count the numbers of `1`, `4`, `7` and `8`, in the output values.

### Solution

First of all, let's take a look at the representation of those numbers on a four-digit seven-segment display.
To represent the number `1`, we need 2 segments. For the number `4`, we need 4. For the number `7`, we need 3. And for the number `8`, we need all 7 of them.

So we start by creating a class which is able to give us this information:
```cpp
class SignalPattern
{
public:
    constexpr SignalPattern(std::string_view message_) : message(message_) {}

    constexpr bool isNumber1() const { return message.size() == 2; }
    constexpr bool isNumber4() const { return message.size() == 4; }
    constexpr bool isNumber7() const { return message.size() == 3; }
    constexpr bool isNumber8() const { return message.size() == 7; }

private:
    std::string_view message;
};
```

Now that we can identify the number we are looking for in a pattern, we need to be able to handle a message.
A message will have the test values and the output values, and will be able to give us the numbers of elements we are looking for, in the output values.

```cpp
struct Message
{
    int getNumberOf1_4_7_8_inOuputs() const {
        return std::accumulate(std::begin(ouputValues), std::end(ouputValues), 0, [](const auto sum, const auto& signalPattern){
            return sum + ((signalPattern.isNumber1() or signalPattern.isNumber4() or signalPattern.isNumber7() or signalPattern.isNumber8()) ? 1 : 0);
        });
    }

    std::array<SignalPattern, 10> testValues;
    std::array<SignalPattern, 4> ouputValues;
};
```

Then, with this class representing a message, we can easily create our input:

```cpp
constexpr std::array<Message, 200> input {
Message {{{{"dbc"}, {"gfecab"}, {"afcdg"}, {"dfebcag"}, {"bd"}, {"dgbe"}, {"bcaeg"}, {"dcefab"}, {"ecgadb"}, {"agcbd"}}},{{{"acdgb"}, {"gbcda"}, {"gdecfba"}, {"bacge"}}}},
/*  ...  */
Message {{{{"dabfegc"}, {"dfegb"}, {"cbgf"}, {"cf"}, {"fdebgc"}, {"dcbaef"}, {"gcfed"}, {"adecg"}, {"fbdgea"}, {"fce"}}},{{{"cf"}, {"gbfc"}, {"fgcb"}, {"begcadf"}}}}
};
```

And finally, we can compute the solution to this problem by calculating the sum of the numbers we are looking for represented in each message:
```cpp
int main()
{
    const auto result = std::accumulate(std::begin(input), std::end(input), 0, [](const auto sum, const auto& message){
        return sum + message.getNumberOf1_4_7_8_inOuputs(); });
    std::cout << "The solution is: " << result << std::endl;
    return 0;
}
```

<details>
  <summary>[Spoiler] Problem Answer</summary>

The puzzle answer was 476.
</details>

The standard function [std::accumulate](https://en.cppreference.com/w/cpp/algorithm/accumulate) really carried us in this part of the problem, so I really encourage you to go take a look to its [documentation](https://en.cppreference.com/w/cpp/algorithm/accumulate) 😉

## Part 2

### Problem

For the second part, we now need to decode completely the output values !
Each output value being composed of 4 patterns, we are going, for each message, retrieve the 4 digit number sent by the submarine, and sum all those number to solve the problem of the day 🙂

### Solution

First of all, we have to modify the `SignalPattern` class that we have created in the previous part.
We are going to add a few functions which will help us deduce the other numbers of the message:

```cpp
class SignalPattern
{
public:
    /* ... */
    bool operator==(const SignalPattern& other) const {
        auto messageCopy = std::string(message);
        auto otherMessageCopy = std::string(other.message);

        std::sort(std::begin(messageCopy), std::end(messageCopy));
        std::sort(std::begin(otherMessageCopy), std::end(otherMessageCopy));

        return messageCopy == otherMessageCopy;
    }

    int getNumberOfCommonSegment(const SignalPattern& other) const {
        return std::accumulate(std::begin(message), std::end(message), 0, [&other](const auto sum, const auto& c){ return sum + (other.message.find(c) != std::string_view::npos ? 1 : 0); });
    }

    constexpr bool isNumber0_6_or_9() const { return message.size() == 6; }
    constexpr bool isNumber2_3_or_5() const { return message.size() == 5; }
    /* ... */
};
```

In this class, the `operator==` will help us compare the pattern once the number they represent is identified; the function `getNumberOfCommonSegment` will allow us to compare the number of segment that 2 pattern have in common, which will come very handy to deduce the number that a pattern represents with the information of the other patterns in the message; and the two last functions allows us to split the unknown pattern in 2 groups depending on the number of segments they use.

Now that we have adapted this first class, let's update the class `Message`.
This class only has one new method, but this one is fairly big !

```cpp
void Message::orderTestValues() {
    for(auto i=0; i<testValues.size(); ++i) {
        if(testValues[i].isNumber1()) { std::swap(testValues[i], testValues[1]); if(i < 1) { --i; } }
        else if(testValues[i].isNumber4()) { std::swap(testValues[i], testValues[4]); if(i < 4) { --i; } }
        else if(testValues[i].isNumber7()) { std::swap(testValues[i], testValues[7]); if(i < 7) { --i; } }
        else if(testValues[i].isNumber8()) { std::swap(testValues[i], testValues[8]); if(i < 8) { --i; } }
    }
    for(auto i=0; i<testValues.size(); ++i) {
        if(testValues[i].isNumber2_3_or_5()) { 
            if(testValues[i].getNumberOfCommonSegment(testValues[1]) == 2) {
                std::swap(testValues[i], testValues[3]);
                if(i < 3) { --i; }
                continue;
            }
            else if(testValues[i].getNumberOfCommonSegment(testValues[4]) == 2) {
                std::swap(testValues[i], testValues[2]);
                if(i < 2) { --i; }
                continue;
            }
            else {
                std::swap(testValues[i], testValues[5]);
                if(i < 5) { --i; }
            }
        }
    }
    for(auto i=0; i<testValues.size(); ++i) {
        if(testValues[i].isNumber0_6_or_9()) { 
            if(testValues[i].getNumberOfCommonSegment(testValues[3]) == 5) {
                std::swap(testValues[i], testValues[9]);
                if(i < 9) { --i; }
                continue;
            }
            if(testValues[i].getNumberOfCommonSegment(testValues[5]) == 4) { 
                std::swap(testValues[i], testValues[0]);
                continue;
            }
            if(testValues[i].getNumberOfCommonSegment(testValues[1]) == 1) {
                std::swap(testValues[i], testValues[6]);
                if(i < 6) { --i; }
                continue;
            }
            assert(false);
        }
    }
}
```

Let's explain this function a little bit! Its purpose is to order the pattern in the test values to place the pattern at the index matching the number it represents. Since we know how to identify the pattern for the numbers `1`, `4`, `7` and `8`, we start by putting them in the right place, in the first `for` loop.

Then, we identify the numbers `2`, `3` and `5` in the second for loop. To do so, I have noticed that the pattern of the number `3` has 2 common segments with the number `1`, while the pattern for the 2 other numbers don't. By being able to differentiate the pattern of one number to the pattern of the other one was key to know which number each pattern represent. With the same logic, I was able to find that the pattern of the number `2` has 2 common segments with the number `4` while the other numbers don't. And finally, the last pattern of this size can only be the number `5`.

And now, we can similarly identify the numbers `0`, `6` and `9`, since the pattern of the number `9` has 5 common segments with the number 3, the pattern of the number `0` has 4 common segments with the number `5` and the pattern of the number `6` has 1 common segment with the number 1. 😊

The only thing we have know left is the `main` function to compute the solution of the problem.

```cpp
int main()
{
    auto messages = input;
    for(auto& message : messages) { message.orderTestValues(); }

    auto result{0};
    for(const auto& message: messages) {
        std::string value;
        for(const auto& outputValue : message.outputValues) {
            auto it = std::find(std::begin(message.testValues), std::end(message.testValues), outputValue);
            assert(it != std::end(message.testValues));
            auto number = std::distance(std::begin(message.testValues), it);
            value += std::to_string(number);
        }
        result += std::stoi(value);
    }

    std::cout << "The solution is: " << result << std::endl;
    return 0;
}
```

So, first of all, we identify the number related to each pattern by running the method `orderTestValues`, on all the message.
Then, we reconstruct the output `value` by [find](https://en.cppreference.com/w/cpp/algorithm/find)ing the pattern of each of its numbers in the message's `testValues`.
Finally each time we reconstruct one output `value`, we sum it to the `result` variable which we will display at the end of the program.

And voilà ! 🙂

<details>
  <summary>[Spoiler] Problem Answer</summary>

The puzzle answer was 1011823.
</details>

## Other Solutions

Like last week, after solving the problem, I went on the [Advent Of Code subreddit](https://www.reddit.com/r/adventofcode/) to see how some other people have solved this day problem, and, like the last time, I found some really interesting solutions!

The first I want to mention is a [great visual explaination](https://i.imgur.com/AlbUK39.png) of the solution I went for, by [@mathik](https://www.reddit.com/user/mathik).

Then, I found those 3 really satisfying visualization of the problem solved:
- [Visualization created in Golang](https://www.reddit.com/r/adventofcode/comments/rbl3tn/2021_day_8_part_b_golang_grinding_out_those/), by [@asymmetricia](https://www.reddit.com/user/asymmetricia/)
- [Visualization created in Clojure](https://www.reddit.com/r/adventofcode/comments/rbp02i/2021_day_8_unscrambling_segments/), by [@nbardiuk](https://www.reddit.com/user/nbardiuk/)
- [Visualization created in Python](https://www.youtube.com/watch?v=BHtGCi1aiMI), by [@p88h](https://www.reddit.com/user/p88h/)

And finally, there is 2 great videos explaining some solutions to this problem, and one really informative Reddit thread on the possible approach to this problem, that I really enjoyed:
- [Python solution explained](https://youtu.be/TG8nDpBPc3A) by [Jeffery Frederic](https://www.youtube.com/channel/UCrPkSmMdTzd4E1E3S7pCxjg)
- [Fingerprinting technique explained](https://youtu.be/mD-omld3dVQ) by [Sanguine Whale](https://www.youtube.com/channel/UC8tXN0XYMpfscUqGMvGocXQ)
- [Reddit thread](https://www.reddit.com/r/adventofcode/comments/rc5s3z/2021_day_8_part_2_a_simple_fast_and_deterministic/) initiated by [@mnufat17](https://www.reddit.com/user/mnufat17/)

## Conclusion

You can note that the solutions written in this post, don't include all the sources to make running programs, but only the interesting part of the sources to solve this problem.
If you want to see the programs from end to end, you can go on my [GitHub account](https://github.com/Xav83/adventofcode2021/tree/main/Day%208), explore the full solution, add comments or ask questions if you want to, on the platform you read this article, it will also help me improve the quality of my articles.

--------------

Thank you all for reading this article,
And until my next article, have a splendid day 😉

![Advent Of Code 2021](https://raw.githubusercontent.com/Xav83/Xav83.github.io/master/res/Advent%20Of%20Code/2021/Screenshot%20Day%208.png)

## Interesting links

- [constexpr documentation](https://en.cppreference.com/w/cpp/language/constexpr)
- [std::array documentation](https://en.cppreference.com/w/cpp/container/array)
- [std::accumulate documentation](https://en.cppreference.com/w/cpp/algorithm/accumulate)
- [std::swap documentation](https://en.cppreference.com/w/cpp/algorithm/swap)
- [std::find documentation](https://en.cppreference.com/w/cpp/algorithm/find)
- [std::distance documentation](https://en.cppreference.com/w/cpp/iterator/distance)
- [std::to_string documentation](https://en.cppreference.com/w/cpp/string/basic_string/to_string)
- [std::stoi documentation](https://en.cppreference.com/w/cpp/string/basic_string/stol)
- [lambda documentation](https://en.cppreference.com/w/cpp/language/lambda)
- [range-based for loop documentation](https://en.cppreference.com/w/cpp/language/range-for)
- [Advent of Code - Day 8](https://adventofcode.com/2021/day/8)
- [10xlearner website](www.10xlearner.com), [Twitter](https://twitter.com/10xLearner), [Dev.to](https://dev.to/10xlearner), [CodeNewbie](https://community.codenewbie.org/xav83), [Medium](https://medium.com/@xavier-jouvenot), [GitHub](https://github.com/Xav83/)
