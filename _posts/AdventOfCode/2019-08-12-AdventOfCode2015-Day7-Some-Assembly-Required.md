# Advent Of Code - Some Assembly Required - Puzzle 7

Hello ! I'm Xavier Jouvenot and here is the seventh part of a long series on [Advent Of Code](https://adventofcode.com). You can find the previous part [here](2019-07-05-AdventOfCode2015-Day6-Probably-A-Fire-Hazard.md)

For this new post, we are going to solve the problem from the 7th December 2015, named "Some Assembly Required".
The solution I will propose in C++, but the reasoning can be applied to other languages.

## Part 1

### The Problem

The full version of this problem can be found directly on the [Advent of Code website](https://adventofcode.com/2015/day/7), I will only describe the essence of the problem here:

Santa offered to little Bobby a set of wires and bitwise logic gates, who needs our help to assemble the circuit since he is too young to achieve it on his own.

Each wire is identified by some lowercase letters and can carry a 16-bit signal, and can only get a signal from one source, but can provide its signal to multiple destinations. The signal of a wire is provided to each wire by a gate, another wire, or some specific value. And finally, a gate provides no signal until all of its inputs have a signal.

For example:
```
123 -> x // Attribution of the signal to the wire
456 -> y // Attribution of the signal to the wire
x AND y -> d // Bitwise AND operator
x OR y -> e // Bitwise OR operator
x LSHIFT 2 -> f // Left Shifting
y RSHIFT 2 -> g // Right Shifting
NOT x -> h // Bitwise complement
NOT y -> i // Bitwise complement
```
results in
```
x: 123
y: 456
d: 72
e: 507
f: 492
g: 114
h: 65412
i: 65079
```

In this part, we will have to find the value of the signal in the wire `a`

### Solution

First of all, this problem is totally on a different level from the previous one. So let's start with it.

#### Bit operations

For this problem, we will need some operator on bits.
Hopefully, C++ already defines the one we need \o/

Here they are:
- `&` is the `AND` operator
- `|` is the `OR` operator
- `<<` is the `LSHIFT` operator
- `>>` is the `RSHIFT` operator
- `~` is the `NOT` operator

I encourage you to check the [std documentation](https://en.cppreference.com/w/cpp/language/operator_arithmetic) about those operators.

#### The Types

Let's starts with the types/object we will need.
We have the `Signal` which is going to be a `uint16_t` since the signal is on 16 bits.
```c++
using Signal = uint16_t;
```

We have the `Wire` which is an object which have a name and can have a Signal. Here is what the header could look like.
```c++
class Wire
{
public:
    explicit Wire (const std::string& name_);
    ~Wire ();

    bool hasSignal () const;

    void setSignal(const Signal value_);
    Signal getSignal() const;

    std::string getName() const;
private:
    std::string name;
    bool hasSignalValueBeenSet{false};
    Signal value{0};
};
```

Now that we have the Signal and the Wire, the last object we need is the operation allowing us to calculate the value of the signal for each wire. For that, we have 5 kind of Operations :
```c++
enum class OperationType
{
    AND, // x AND y -> z
    OR, // x OR y -> z
    LSHIFT, // x LSHIFT 2 -> y
    RSHIFT, // x RSHIFT 2 -> y
    NOT, // NOT x -> y
    ATTRIBUTION, // 123 -> x
};
```
Which means we have to set the behavior for each operations with a common interface for each kind of operation:
```c++
class Operation
{
public:
    virtual Wire& getResultWire () = 0;
    virtual bool canBeExecuted () const = 0;
    virtual bool isExecuted () const = 0;
    virtual void execute () = 0;
};
```

__Edit__ : Note that an "output Wire" can be integrated directly in the `Operation` class, since all the operation can only have one output.

Each specific operation will inherit from this class and override the function to specify its behavior.
For example :

```c++
class LShift : public Operation
{
public:
    LShift (Wire& input_, size_t number_, Wire& output_) : input(input_), number(number_), output(output_) {}
    ~LShift () = default;

    Wire& getResultWire () override
    {
        return output;
    }

    bool canBeExecuted () const override
    {
        return input.hasSignal();
    }

    bool isExecuted () const override
    {
        return hasBeenExecuted;
    }

    void execute () override
    {
        output.setSignal (input.getSignal() << number);
        hasBeenExecuted = true;
    }

private:
    Wire& input, &output;
    size_t number{0};
    bool hasBeenExecuted{false};
};
```

If you want to see all the implementation of the Operation, I invite you to go to my [Github](https://github.com/Xav83/AdventOfCode/tree/2015.07/2015/Day7).

#### The program

Now that we have our main classes, we can extract the data from the puzzle input with some code like:

```c++
class Wires; // vector of Wire with some specific behavior
class Operations; // vector of Operation with a method "run" to run all the operations

foreachLineIn(fileContent, [&wires](const std::string& line)
{
    const auto operationType = getOperationFromInstruction (line);
    auto& wiresFromInstruction = getWiresNameInInstruction (line);
    for(auto& w : wiresFromInstruction.getWires())
    {
        wires.addNewWire(w); // Only adds wire if it doesn't already exist
    }
});

foreachLineIn(fileContent, [&wires, &operations](const std::string& line)
{
    const auto operationType = getOperationFromInstruction (line);
    auto& wiresFromInstruction = getWiresNameInInstruction (line);
    operations.addNewOperation(operationType, wires, wiresFromInstruction, line);
});
```

Now that we have all the data, we "just" have to it run, ... well I mean `operations.run()`.
```c++
void Operations::run()
{
    while (std::any_of(std::begin(operations), std::end(operations), [](const auto& operation)
    {
        return !operation->isExecuted();
    }))
    {
        for(auto& operation : operations)
        {
            if(!operation->isExecuted() && operation->canBeExecuted())
            {
                operation->execute();
            }
        }
    }
}
```

And voilà, each wire has its signal. All there is now to do is to use `std::find_if` to get the Wire named `a` to solve this part.

## Part 2

### The Problem

For this part, we have to take the signal you got on wire a, override wire b to that signal, and reset the other wires (including wire a) and find the value of the signal in the wire a again.

### Solution

Well... now that we have a software able to find the value of the wire `a`, all we have to do is modify the line giving its value to the wire b and it run on the program again, and get the result, right ?

Exactly, it work \o/ Love the reusability of the software ❤️

## Conclusion

You can note that the solutions, written in this post, don't include all the sources to make running programs, but only the interesting part of the sources to solve this problem.
If you want to see the programs from end to end, you can go on my [GitHub account](https://github.com/Xav83/AdventOfCode/tree/2015.07/2015/Day7), explore the full solution, add comments or ask questions if you want to.

Here is the list of std method that we have used, I can't encourage you enough to look at their definitions :

- [using](https://en.cppreference.com/w/cpp/language/type_alias)
- [std::any_of](https://en.cppreference.com/w/cpp/algorithm/all_any_none_of)

Thanks for you reading, hope you liked it 😃
And until next part, have fun learning and growing.

