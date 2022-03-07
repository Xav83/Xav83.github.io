# Advent Of Code 2021 - Syntax Scoring - Puzzle 10

Hello ! I'm Xavier Jouvenot and here is the 10th part of a long series on [Advent Of Code 2021](https://adventofcode.com).

For this new post, we are going to solve the problem from the 10th December 2021, named "Syntax Scoring".
The solution I will propose in C++, but the reasoning can be applied to other languages.

_Self promotion_:
Here are a few social networks where you can follow me and check my work as a programmer and a writer 😉 
[Personal blog](www.10xlearner.com), [Twitter](https://twitter.com/10xLearner), [Dev.to](https://dev.to/10xlearner), [CodeNewbie](https://community.codenewbie.org/xav83), [Medium](https://medium.com/@xavier-jouvenot), [GitHub](https://github.com/Xav83/)

## Part 1

### The Problem

The full version of this problem can be found directly on the [Advent of Code website](https://adventofcode.com/2021/day/10), I will only describe the essence of the problem here:

Our submarine wants to give us some information, but the navigation subsystem is broken and gives us a wonderful message:
> Syntax error in navigation subsystem on line: all of them

We learn that navigation subsystem syntax is made of several lines containing chunks, and each chunk must be open and close with one of four legal pairs of matching characters: '(' and ')', '{' and '}', '[' and ']', '<' and '>'. A chunk can contains several chunks, and one chunk can follow another one, but one chunk can not start inside one chunk and finish in another one.

We can, for example, have a navigation subsystem looking like:

```
[({(<(())[]>[[{[]{<()<>>
[(()[<>])]({[<{<<[]>>(
{([(<{}[<>[]}>{[]{[(<()>
(((({<>}<{<{<>}{[]{[]{}
[[<[([]))<([[{}[[()]]]
[{[{({}]{}}([{[{{{}}([]
{<[[]]>}<{[{[{[]{()[[[]
[<(<(<(<{}))><([]([]()
<{([([[(<>()){}]>(<<{{
<{([{{}}[<[[[<>{}]]]>[]]
```

So we need to check the syntaxe given, identify the corrupted lines, and use the first illegal character of each line and compute a score which is going to be the answer of this part.

### Solution

Basically, we are trying to find errors in a very limited language composed only of parenthesis, brackets, square brackets and single angle quotation marks. 

First of all, we need some utility function to help us associate the character of a chunk to each other:
```cpp
constexpr bool isOpeningCharacter(char c)
{
    return c == '(' or c == '[' or c == '{' or c == '<';
}

constexpr char getMatchingCharacterOf(char c)
{
    if(c == '(') { return ')'; }
    if(c == ')') { return'('; }
    if(c == '[') { return ']'; }
    if(c == ']') { return '['; }
    if(c == '{') { return '}'; }
    if(c == '}') { return '{'; }
    if(c == '<') { return '>'; }
    if(c == '>') { return '<'; }
    assert(false);
    return '\0';
}
```

Those functions are pretty explicit about what they are doing, as the first one tells us if a character is an opening character of a chunk, and the second one tells us that is the character we should have on the other side of the chunk, for a given character.

Before going to the main code, we also need another function to know the score of each characters:
```cpp
constexpr int getCharacterScore(char c)
{
    if(c == ')') { return 3; }
    if(c == ']') { return 57; }
    if(c == '}') { return 1197; }
    if(c == '>') { return 25137; }
    assert(false);
    return 0;
}
```

Still pretty trivial 😉

Now, we can define the input we are going to use, with a combination of a [std::array](https://en.cppreference.com/w/cpp/container/array) and [std::string_view](https://en.cppreference.com/w/cpp/string/basic_string_view):
```cpp
constexpr std::array <std::string_view, 102> input {{
    "([({<(({<[(<([{}<>])<(()<>){<>[]}>>[[{<>{}}[<>{}]][(<>[])<{}<>>]]){[{{[][]}[[]{}]}({{}[]}[{}<>])]({({}{})",
    "([{{[(<{[{{<[{{}[]}{(){}}]{[{}{})([]())}>}(({{{}}{<>{}}}{(()[])(()<>)}){<<{}()><[][]>>})}(<<<(<",
    /* ... */
    "{[{<[([[[[({{[{}{}]{[]()}}<([][])[[][]]>}{({<>()}(<>[]))[({}<>)[<>{}]]})]]]{{{{[[[{}[]](()<>)]["
}};
```

Finally, we can write the `main` function:
```cpp
int main()
{
    std::stack<char> openChunks;
    auto score{0};
    for(const auto& line : input)
    {
        for(const auto& character : line)
        {
            if(isOpeningCharacter(character))
            {
                openChunks.push(character);
                continue;
            }
            if(openChunks.top() == getMatchingCharacterOf(character))
            {
                openChunks.pop();
                continue;
            }
            score += getCharacterScore(character);
            break;
        }
    }

    std::cout << "The solution is: " << score << std::endl;
    return 0;
}
```

If you know what a [stack](https://en.wikipedia.org/wiki/Stack_(abstract_data_type)) is, and how it works, this code should be fairly easy to understand.

But those who don't know yet (and I really encourage you to check out what the stack structure is, as you are probably going to need it in your programming journey 😉), a stack is a collection of elements where each element can be pushed in and popped out following one simple rule: The last element which have been added to the collection will be the first to be removed.
So, if you add the elements 'a', 'b' and 'c' in a stack (in that specific order), then, if you tell the stack to remove one element, the element 'c' (the last one added), will be removed, and if you asl the stack to remove yet another one, the element 'b' (the last one added in the remaining elements) will be removed.

So in this main function, we instantiate a [std::stack](https://en.cppreference.com/w/cpp/container/stack), which is the native implementation of the stack data structure, in C++. Then, we iterate though the input, looking at each character: adding it in the stack if it is an opening character, and removing the last element of the stack if it is the matching character (the closing character of the opening one stored). In the case where the character is a closing character and it doesn't match the last opening character added to the stack, then, we have find a syntax error, and we can increment the score before moving on to the next line. Finally, one we have iterate through each line, we can displayed the score computed when iterating through the input.

<details>
  <summary>[Spoiler] Problem Answer</summary>

The puzzle answer was 374061.
</details>

## Part 2

### Problem

Now that we are able to find corrupted lines, we need to deal with the incomplete one.
And to do it correctly, we need to figure out the missing sequence of closing characters which will make each line correct.

Moreover, there is a specific way to calculate a score of each sequence found!
So we need to calculate this score for each line, order the line depending on this score and find the middle score of the input.

### Solution

This time, we are only going to define one trivial function before diving into the `main`:
```cpp
constexpr int getCharacterScoreOfIncompleteLines(char c)
{
    if(c == ')') { return 1; }
    if(c == ']') { return 2; }
    if(c == '}') { return 3; }
    if(c == '>') { return 4; }
    assert(false);
    return 0;
}
```

This function is only going to help use simplify the calculation of the score of a line.

Now the `main` function !! 🙂
```cpp
int main()
{
    std::vector<size_t> scores;
    for(const auto& line : input)
    {
        std::stack<char> openChunks;
        bool isCorruptedLine{false};
        for(const auto& character : line)
        {
            if(isOpeningCharacter(character))
            {
                openChunks.push(character);
                continue;
            }
            if(openChunks.top() == getMatchingCharacterOf(character))
            {
                openChunks.pop();
                continue;
            }
            isCorruptedLine = true;
            break;
        }
        if(not isCorruptedLine)
        {
            size_t lineScore{0};
            while (not openChunks.empty())
            {
                lineScore = lineScore * 5 + getCharacterScoreOfIncompleteLines(getMatchingCharacterOf(openChunks.top()));
                openChunks.pop();
            }
            scores.push_back(lineScore);
        }
    }
    std::sort(std::begin(scores), std::end(scores));

    std::cout << "The solution is: " << scores[scores.size() / 2] << std::endl;
    return 0;
}
```

In here, we are starting by doing basically the same thing than in the previous part, but, calculating the score for each character, we only flag the line if this is a corrupted line or not. If this is not a corrupted line, then it is an incomplete one, and the score calculation starts!

To calculate the score, we empty the stack, and for each character we remove, we apply the method given by **Advent Of Code**, meaning, we multiply the current score of the line by `5` and we add the score of the character we have removed from the stack. Once the stack is empty, we can add the score line to the array of scores.

Finally, once we have collected all the lines' scores, we sort them and display the answer of the problem: the score in the middle of all the line's scores.

<details>
  <summary>[Spoiler] Problem Answer</summary>

The puzzle answer was 2116639949.
</details>

*Note:* I had to use `size_t` as the type for the scores since the value were too large for the `int` type (I found out that the hard way 😝)

## Other Solutions

As usual, after solving the problem, I went on the [Advent Of Code subreddit](https://www.reddit.com/r/adventofcode/) to see how some other people have solved this day problem, and, as always, I found some really interesting things! 🙂

The first element I want to mention is this [article](https://pgaleone.eu/tensorflow/2022/01/04/advent-of-code-tensorflow-day-10/) by [Paolo Galeone](https://pgaleone.eu/about/) which is a very interesting one where he solves the problem using TensorFlow and Python !

Then, I found those 2 live-coding sessions where 2 people go through this day's problem and solve it in a specific language. This was very enjoyable to watch and allowed me to learn a little more about those language at the same time:
- ["Solving day 10 of Advent of Code 2021 in JavaScript"](https://www.youtube.com/watch?v=tuH8eO4WWr0), by [thibpat](https://twitter.com/thibpat/)
- ["Advent of Code 2021 Day 10 (Rust)"](https://www.youtube.com/watch?v=Lq6FA2HyuSk), by [Main Gauche](https://twitter.com/maingauchegames)

Finally, as usual, people are really creative and come up with really satisfying visualization for the algorithm solving those problems!
Here are the 3 I personally enjoyed the most 😉
- [Visualization](https://www.reddit.com/r/adventofcode/comments/rd2zjv/2021_day_10_syntax_scoring/) by [@jjjefff](https://www.reddit.com/user/jjjefff/)
- [Visualization in TypeScript](https://www.reddit.com/r/adventofcode/comments/rd9k82/2021_day_10_typescript_heres_my_take_of/) by [@TenViki](https://www.reddit.com/user/TenViki/)
- [Visualization using Pygame](https://www.youtube.com/watch?v=Z53G_pm1cVg) by [@p88h](https://www.youtube.com/channel/UCEdOvQjF7qZPFcv5M-luxPw)

## Conclusion

You can note that the solutions written in this post, don't include all the sources to make running programs, but only the interesting part of the sources to solve this problem.
If you want to see the programs from end to end, you can go on my [GitHub account](https://github.com/Xav83/adventofcode2021/tree/main/Day%2010), explore the full solution, add comments or ask questions if you want to, on the platform you read this article, it will also help me improve the quality of my articles.

--------------

Thank you all for reading this article,
And until my next article, have a splendid day 😉

![Advent Of Code 2021](https://raw.githubusercontent.com/Xav83/Xav83.github.io/master/res/Advent%20Of%20Code/2021/Screenshot%20Day%2010.png)

## Interesting links

- [constexpr documentation](https://en.cppreference.com/w/cpp/language/constexpr)
- [std::array documentation](https://en.cppreference.com/w/cpp/container/array)
- [std::string_view documentation](https://en.cppreference.com/w/cpp/string/basic_string_view)
- [std::stack documentation](https://en.cppreference.com/w/cpp/container/stack)
- [lambda documentation](https://en.cppreference.com/w/cpp/language/lambda)
- [Advent of Code - Day 10](https://adventofcode.com/2021/day/10)
- [10xlearner website](www.10xlearner.com), [Twitter](https://twitter.com/10xLearner), [Dev.to](https://dev.to/10xlearner), [CodeNewbie](https://community.codenewbie.org/xav83), [Medium](https://medium.com/@xavier-jouvenot), [GitHub](https://github.com/Xav83/)
