# Advent Of Code - Aunt Sue - Puzzle 16

Hello ! I'm Xavier Jouvenot and here is the part sixteenth of a long series on [Advent Of Code](https://adventofcode.com). You can find the previous part [here](2019-10-21-AdventOfCode2015-Day15-Science-for-Hungry-People.md)

For this new post, we are going to solve the problem from the 16th December 2015, named "Aunt Sue".
The solution I will propose in C++, but the reasoning can be applied to other languages.

## Part 1

### The Problem

The full version of this problem can be found directly on the [Advent of Code website](https://adventofcode.com/2015/day/16), I will only describe the essence of the problem here:

You have received a wonderful gift, the My First Crime Scene Analysis Machine (or MFCSAM), from your Aunt Sue, and you want to thank her.
But, since you are from the biggest family in the word, you have 500 Aunts named Sue !

So you are wondering which of your Aunt Sue send you this wonderful gift !
But luckily for you, the MDCSAM can detect a few specific compounds in a given sample, as well as how many distinct kinds of those compounds there are.
According to the instructions, these are what the MFCSAM can detect:

- `children`, by human DNA age analysis.
- `cats`. It doesn't differentiate individual breeds.
- Several seemingly random breeds of dog: `samoyeds`, `pomeranians`, `akitas`, and `vizslas`.
goldfish. No other kinds of fish.
- `trees`, all in one group.
- `cars`, presumably by exhaust or gasoline or something.
- `perfumes`, which is handy, since many of your Aunts Sue wear a few kinds.

And it prints out the following message for the Aunt Sue that you are looking for:
```
children: 3
cats: 7
samoyeds: 2
pomeranians: 3
akitas: 0
vizslas: 0
goldfish: 5
trees: 3
cars: 2
perfumes: 1
```

How this machine is able to know that is truly a mystery, but why not 😝

So you make a list of the things you can remember about each Aunt Sue.
Things missing from your list aren't zero - you simply don't remember the value.

So now, you hopefully can find which Aunt Sue give you the gift.

### Solution

First, let's describe what are the characteristics of each Aunt.
Each Aunt have 10 compounds : children, cats, samoyeds, pomeranians, akitas, vizslas, goldfish, trees, cars and perfumes. And some of them won't be specified. We can implement those characteristics like so:

```c++
using CompoundValue = std::optional<size_t>;
using Compound = std::pair<std::string, CompoundValue>;

class Aunt {
  std::array<Compound, 10> compounds;
};
```

In this code, an aunt have 10 compounds in the [array](https://en.cppreference.com/w/cpp/container/array), and each compound has a name and a value, but this value is [optional](https://en.cppreference.com/w/cpp/utility/optional). Respecting all the requirements.

Now, we need to be able to "construct" an Aunt with only the known elements. To do so, we are going to add constructors.

```c++
Aunt::Aunt() {
  compounds[0].first = "children";
  compounds[1].first = "cats";
  compounds[2].first = "samoyeds";
  compounds[3].first = "pomeranians";
  compounds[4].first = "akitas";
  compounds[5].first = "vizslas";
  compounds[6].first = "goldfish";
  compounds[7].first = "trees";
  compounds[8].first = "cars";
  compounds[9].first = "perfumes";
}

Aunt::Aunt(std::vector<Compound> knownElements) : Aunt() {
  for (const auto &knownElement : knownElements) {
    auto compound =
        std::find_if(std::begin(compounds), std::end(compounds),
                     [&knownElement](const auto &compound) {
                       return compound.first == knownElement.first;
                     });
    assert(compound != std::end(compounds));
    compound->second = knownElement.second;
  }
}
```

So, first, we have the default constructor which have the only purpose to adds the name of each compounds, since we already know them.
This constructor can be private, since, we will always try to "create" an Aunt with the knows compounds about her.

And this is exactly what the second constructor does.
It starts by calling the default constructor to set the names of the compounds, before setting the value of the knows elements. To do so, for each of the known compounds, it looks for matching on in the array and sets it's value.
At the end of this constructor, we have an aunt in which only the known compounds have a value.

We can now construct the 500 Aunts.
I won't detail how I extracted the information from the input file, since the input given by Advent Of Code is very specific, but you can find the code [here](https://github.com/Xav83/AdventOfCode/blob/2015.16/2015/Day16/src/Aunt.cpp#L32), if you want 😉

Once we have "created" all our aunts, we then must find which one is the person who has sent you the gift.
The code to do so is pretty straight forward and look like that:

```c++
std::vector<Aunt> aunts;

// Creation of the 500 aunts
foreachLineIn(fileContent, [&aunts](const std::string &line) {
    aunts.emplace_back(extractCompounds(line));
});

std::array<Compound, 10> ticketTape{
    Compound{"children", 3}, Compound{"cats", 7},
    Compound{"samoyeds", 2}, Compound{"pomeranians", 3},
    Compound{"akitas", 0},   Compound{"vizslas", 0},
    Compound{"goldfish", 5}, Compound{"trees", 3},
    Compound{"cars", 2},     Compound{"perfumes", 1}};

auto aunt = std::find_if(
    std::begin(aunts), std::end(aunts), [&ticketTape](const auto &aunt) {
    const auto &auntCompounds = aunt.getCompounds();
    for (auto i = size_t{0}; i < auntCompounds.size(); ++i) {
        if (auntCompounds[i].second.has_value() &&
            auntCompounds[i].second.value() != ticketTape[i].second) {
        return false;
        }
    }
    return true;
    });

const auto numberOfAuntSue = std::distance(std::begin(aunts), aunt) + 1;
```

Let's go into the details!
After creating the aunts, we create a ticket tape, with the message given by the MDCSAM (do you remember what this acronym stand for ? 😜) corresponding to the compounds of the Aunt Sue to find.

Then, to **find** her, we use the standard algorithm [std::find_if](https://en.cppreference.com/w/cpp/algorithm/find), looking for the aunt which have all its known element matching the value the one on the ticket tape.

At the end of the [std::find_if](https://en.cppreference.com/w/cpp/algorithm/find) instruction, we can count the position of this Aunt in the vector, since the problem demands this information.

And voilà, we have found our Aunt Sue, and we can send her our gift card 🙂

## Part 2

### Problem

Just before you send your thank you note, you look at the MFCSAM's instructions and see that you miss some details.

In fact, the value given in the message of the MFCSAM are the exact value.
In particular, the cats and trees readings indicates that there are greater than that many, due to the unpredictable nuclear decay of cat dander and tree pollen, while the pomeranians and goldfish readings indicate that there are fewer than that many, due to the modial interaction of magnetoreluctance.

So whatever the reason, we must now found the real Aunt Sue than send us the gift !

### Solution

To do so, all we have to change is the content of the std::find_if used to find the right Aunt Sue:

```c++
auto aunt = std::find_if(
    std::begin(aunts), std::end(aunts), [&ticketTape](const auto &aunt) {
    const auto &auntCompounds = aunt.getCompounds();
    for (auto i = size_t{0}; i < auntCompounds.size(); ++i) {
        if (!auntCompounds[i].second.has_value()) {
        continue;
        }
        if (auntCompounds[i].first == "cats" ||
            auntCompounds[i].first == "trees") {
        if (auntCompounds[i].second.value() <= ticketTape[i].second) {
            return false;
        }
        continue;
        }
        if (auntCompounds[i].first == "pomeranians" ||
            auntCompounds[i].first == "goldfish") {
        if (auntCompounds[i].second.value() >= ticketTape[i].second) {
            return false;
        }
        continue;
        }
        if (auntCompounds[i].second.has_value() &&
            auntCompounds[i].second.value() != ticketTape[i].second) {
        return false;
        }
    }
    return true;
    });
```

So, what are we exactly doing in plain English:

For each of our aunts, we look at each of her compounds.
If the value is unknown, we ignore it.

But if it is known, then we check if it's a particular compound with a specific behavior.
If not, we act like before, if not, we check is the value is good depending on the instructions.

If everything is good, then, we have the right Aunt Sue, if not, we keep looking for her, until we found her.

And finally, we can send her the thank you note 😄

## Conclusion

You can note that the solutions, written in this post, don't include all the sources to make running programs, but only the interesting part of the sources to solve this problem.
If you want to see the programs from end to end, you can go on my [GitHub account](https://github.com/Xav83/AdventOfCode/tree/2015.16/2015/Day16), explore the full solution, add comments or ask questions if you want to, on the platform you read this article, it will also help me improve the quality of my articles.

Here is the list of elements that we have used, I can't encourage you enough to look at their definitions :

- [std::optional](https://en.cppreference.com/w/cpp/utility/optional)
- [std::pair](https://en.cppreference.com/w/cpp/utility/pair)
- [std::string](https://en.cppreference.com/w/cpp/string/basic_string)
- [std::array](https://en.cppreference.com/w/cpp/container/array)
- [std::vector](https://en.cppreference.com/w/cpp/container/vector)
- [std::find_if](https://en.cppreference.com/w/cpp/algorithm/find)
- [std::begin](https://en.cppreference.com/w/cpp/iterator/begin)
- [std::end](https://en.cppreference.com/w/cpp/iterator/end)
- [std::distance](https://en.cppreference.com/w/cpp/iterator/distance)

Thanks for you reading, hope you liked it 😃

And until next part, have fun learning and growing.
