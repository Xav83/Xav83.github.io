# Advent Of Code 2021 - Giant Squid - Puzzle 4

Hello ! I'm Xavier Jouvenot and here is the 4th part of a long series on [Advent Of Code 2021](https://adventofcode.com).

For this new post, we are going to solve the problem from the 4th December 2021, named "Giant Squid".
The solution I will propose in C++, but the reasoning can be applied to other languages.

_Self promotion_: You can find other articles on computer science and programming on my [website](www.10xlearner.com) 😉

## Part 1

### The Problem

The full version of this problem can be found directly on the [Advent of Code website](https://adventofcode.com/2021/day/4), I will only describe the essence of the problem here:

After diving into the decoding the report of the submarine, during the Day 3, we are now deep into the ocean. So deep, that a giant squid attach itself to the submarine and that we are going to play [bingo](https://en.wikipedia.org/wiki/Bingo_(American_version)) with it ! The rule are simple, there are some bingo cards available and we know which numbers are going to be chosen, and the first card to have either a row or a column of number marked (meaning the numbers have been selected during the previous round) win.

In order to make sure that we have the right card, the website ask us to give it a specific number obtained by multiplying the **last number used during the last round** to the sum of the unmarked number of the winning board.

You can look at the example given on [Advent of Code website](https://adventofcode.com/2021/day/4), if you want to look at how those rates are generated from the example.

### Solution

In order to find the solution, I have modified the input manually a little bit.
Indeed, the thing I am interested in is the logic to use to find the solution, not parsing a text file to populate some variables.

So the data and structure we are going to used look like that:
```cpp
constexpr std::array<int, 100> input_numbers{93,35,66, /*...*/ ,89};

struct BoardNumber
{
    constexpr BoardNumber(int number_) : number(number_) {}

    const int number{0}; 
    bool hasBeenFound{false};
};

constexpr auto board_width = 5;
using Board = std::array<std::array<BoardNumber, board_width>, board_width>;
constexpr std::array<Board, 100> input_boards{
    Board {{{14,33,79,61,44}, {85,60,38,13,48}, {51,34,11,19,7}, {21,30,73,6,76}, {41,4,65,18,91}}},
    Board {{{3,82,68,26,93}, {61,90,29,69,92}, {60,94,99,6,83}, {77,80,2,58,55}, {59,65,95,38,62}}},
    Board {{{41,9,73,71,74}, {66,24,45,5,55}, {97,82,53,63,16}, {12,19,88,87,27}, {31,8,75,98,83}}},
    /* ... */
    Board {{{74,95,14,35,49}, {67,66,57,76,88}, {89,71,68,69,48}, {20,70,3,0,12}, {13,21,15,51,24}}}
};
```

The constant `input_numbers` contains the list of number which are going to be picked to play bingo, and the constant `input_boards` contains the array of boards available. Moreover, I have specified a `BoardNumber` type which will store the number on the board, and if we have marked the number, when picked during the game.

Now that we know with what structure we are dealing with, we can start to see how we are going to find our solution to the problem.
The first thing to do, is to simulate the bingo party until we find a winning board:
```cpp
auto unmarkedNumbersSum{0}, lastNumberCalled{0};
auto playingBoards = input_boards;

for(size_t index{0}; index < input_numbers.size(); ++index) {
    lastNumberCalled = input_numbers[index];
    for(auto& board : playingBoards) { updateBoard(board, lastNumberCalled); }

    if(std::any_of(std::begin(playingBoards), std::end(playingBoards), [](const auto& board){ return isAWinningBoard(board); }))
    {
        break;
    }
}
```

With this code, we start by using a `for` loop to go through number picked during the bingo game.
Then, we store the number used, for later use if necessary, and we update all the boards, with the number that has been picked using an `updateBoard` function, that we are going to look into in a little moment.
And finally, we look to see if a board is no winning board with the [std::any_of](https://en.cppreference.com/w/cpp/algorithm/all_any_none_of) C++ function and the function `isAWinningBoard` that we are to look at too. If there is a winning board, we look stop looking for one, and break from the `for` loop, and execute the following code:

```cpp
auto winningBoardIterator = std::find_if(std::begin(playingBoards), std::end(playingBoards), [](const auto& board){ return isAWinningBoard(board); });

for(auto& row : *winningBoardIterator) {
    for(auto& boardNumber: row) {
        if(not boardNumber.hasBeenFound) {
            unmarkedNumbersSum += boardNumber.number;
        }
    }
}

std::cout << "The solution is: " << (lastNumberCalled * unmarkedNumbersSum) << std::endl;
```

All we have to do, now that we have played bingo until a winning board appeared, is to find the winning board with the [std::find_if](https://en.cppreference.com/w/cpp/algorithm/find) function and the function `isAWinningBoard` once more.
Finally, we calculate the sum of the unmarked number of the winning board and we multiply it to the information stored in the `lastNumberCalled` to get the final result, and answer of the problem 🙂👍

<details>
  <summary>[Spoiler] Problem Answer</summary>

The puzzle answer was 27027.
</details>

Before going to the next part, let's take a look to the two functions that we used : `updateBoard` and `isAWinningBoard`
```cpp
void updateBoard(Board& board, int choosenNumber) {
    for(auto& row : board) {
        for(auto& boardNumber: row) {
            if(boardNumber.number == choosenNumber) {
                boardNumber.hasBeenFound = true;
            }
        }
    }
}

bool isAWinningBoard(const Board& board)
{
    for(auto i=0; i<board_width; ++i) {
        if(std::all_of(std::begin(board[i]), std::end(board[i]), [](const auto& boardNumber){ return boardNumber.hasBeenFound; })) {
            return true;
        }
        if(board[0][i].hasBeenFound and board[1][i].hasBeenFound and board[2][i].hasBeenFound and board[3][i].hasBeenFound and board[4][i].hasBeenFound) {
            return true;
        }
    }
    return false;
}
```

The first one update the flag `hasBeenFound` for the number when it matches to the one in parameter.
The second one uses the std function to check if all the elements of one raw have been marked, or if all the element of one column have been marked and return true if this is the case.

## Part 2

### Problem

In order to make the giant squid happy, a new strategy has been adapted: make the squid win, to avoid making it angry.
So we need now, to find the last board which is going to win.

### Solution

The solution for this part is almost the same than for the previous part.
The only modification to do is to not break when the first winning board is found, but to remove the winning board from the list of board until we have only one left, the one we want 😉

```cpp
std::vector<Board> playingBoards(std::begin(input_boards), std::end(input_boards));

/* ... */

    auto winningBoardIterator = std::find_if(std::begin(playingBoards), std::end(playingBoards), [](const auto& board){ return isAWinningBoard(board); });

    if(winningBoardIterator == std::end(playingBoards)) { continue; }
    if(playingBoards.size() == 1) { break; }

    playingBoards.erase(std::remove_if(std::begin(playingBoards), 
                                        std::end(playingBoards),
                                        [](const auto& board){ return isAWinningBoard(board); }),
                        playingBoards.end());
```

<details>
  <summary>[Spoiler] Problem Answer</summary>

The puzzle answer was 36975.
</details>

## Conclusion

You can note that the solutions written in this post, don't include all the sources to make running programs, but only the interesting part of the sources to solve this problem.
If you want to see the programs from end to end, you can go on my [GitHub account](https://github.com/Xav83/adventofcode2021/tree/main/Day%204), explore the full solution, add comments or ask questions if you want to, on the platform you read this article, it will also help me improve the quality of my articles.

--------------

Thank you all for reading this article,
And until my next article, have a splendid day 😉

![Advent Of Code 2021](https://raw.githubusercontent.com/Xav83/Xav83.github.io/master/res/Advent%20Of%20Code/2021/Screenshot%20Day%204.png)

## Interesting links

- [constexpr documentation](https://en.cppreference.com/w/cpp/language/constexpr)
- [std::find_if documentation](https://en.cppreference.com/w/cpp/algorithm/find)
- [std::any_of documentation](https://en.cppreference.com/w/cpp/algorithm/all_any_none_of)
- [std::array documentation](https://en.cppreference.com/w/cpp/container/array)
- [std::vector documentation](https://en.cppreference.com/w/cpp/container/vector)
- [std::vector::erase](https://en.cppreference.com/w/cpp/container/vector/erase)
- [std::remove_if](https://en.cppreference.com/w/cpp/algorithm/remove)
- [lambda](https://en.cppreference.com/w/cpp/language/lambda)
- [range-based for loop documentation](https://en.cppreference.com/w/cpp/language/range-for)
- [Advent of Code - Day 4](https://adventofcode.com/2021/day/4)
- [10xlearner website](www.10xlearner.com)
