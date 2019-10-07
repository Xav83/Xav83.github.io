# Advent Of Code - Knights of the Dinner Table - Puzzle 13

Hello ! I'm Xavier Jouvenot and here is the part thirteen of a long series on [Advent Of Code](https://adventofcode.com). You can find the previous part [here](2019-09-23-AdventOfCode2015-Day12-JSAbacusFramework.io.md)

For this new post, we are going to solve the problem from the 13th December 2015, named "Knights of the Dinner Table".
The solution I will propose in C++, but the reasoning can be applied to other languages.

## Part 1

### The Problem

The full version of this problem can be found directly on the [Advent of Code website](https://adventofcode.com/2015/day/13), I will only describe the essence of the problem here:

For the holiday feast, you need to make the table arrangement, and decide to go for the one which will be optimal to avoid awkward conversations.
For that, you have a list of everyone invited and the amount their happiness would increase or decrease if they were to find themselves sitting next to each other person.
You also have a circular table that will be just big enough to fit everyone comfortably, and so each person will have exactly two neighbors.

For example, with the list below :

```
Alice would gain 54 happiness units by sitting next to Bob.
Alice would lose 79 happiness units by sitting next to Carol.
Alice would lose 2 happiness units by sitting next to David.
Bob would gain 83 happiness units by sitting next to Alice.
Bob would lose 7 happiness units by sitting next to Carol.
Bob would lose 63 happiness units by sitting next to David.
Carol would lose 62 happiness units by sitting next to Alice.
Carol would gain 60 happiness units by sitting next to Bob.
Carol would gain 55 happiness units by sitting next to David.
David would gain 46 happiness units by sitting next to Alice.
David would lose 7 happiness units by sitting next to Bob.
David would gain 41 happiness units by sitting next to Carol.
```

You should find that the arrangement below has the optimal change in happiness equal to `330`:

```
     +41 +46
+55   David    -2
Carol       Alice
+60    Bob    +54
     -7  +83
```

And we need to do that for a much bigger table... Let's go ! 💪

### Solution

We are going to split the solution in two parts : the extraction of the useful data in the attendees list, and the algorithm to find the optimal change in happiness.
Without further due, let's go to the first part.

#### Data Extraction

Before solving anything, we need to extract the useful data from the list of attendees.
So first let's identify them ! In the statement below, there are several useful information:

```
Alice would gain 54 happiness units by sitting next to Bob.
```

There is **Alice** the person which is the main subject of this *relationship*, and **Bob**, the neighbor which will impact Alice happiness. The other information is obviously the amount of happiness **54** and also, the keyword **gain** which indicates an increase in happiness. If it wasn't a **gain**, but a **lose** keyword, then, it would be an decrease of happiness.

That all the information that one line can give us. In fact, the other information needed will be the list of all the attendees.
And now, that we know what we need, let's write the code for it.

First we can extract the happiness, only by getting the first number in the instruction

```c++
using Happiness = int;
Happiness extractHappinessFrom (const std::string& instruction)
{
    std::regex word_regex("[0-9]+");
    auto words_begin = std::sregex_iterator(std::begin(instruction), std::end(instruction), word_regex);
    auto words_end = std::sregex_iterator();

    for (std::sregex_iterator i = words_begin; i != words_end; ++i)
    {
        std::smatch match = *i;
        return static_cast<Happiness>(atoi(match.str().c_str()));
    }
    assert(false);
    return 0;
}
```

Then, we can extract a relationship between the two persons by extracting the happiness and getting the first and the last words of the relations list:

```
std::tuple<PersonName, PersonName, Happiness> extractInformationFrom (const std::string& instruction)
{
    const auto firstSpaceCharacter = std::find_if(std::begin(instruction), std::end(instruction), [](const auto& c){ return c == ' '; });
    const auto lastSpaceCharacter = std::find_if(std::rbegin(instruction), std::rend(instruction), [](const auto& c){ return c == ' '; });

    PersonName fromPersonName (std::begin(instruction), firstSpaceCharacter);
    PersonName toPersonName (lastSpaceCharacter.base()++, std::rbegin(instruction).base());
    toPersonName.pop_back(); // Removes the dot

    auto happiness = extractHappinessFrom (std::string(instruction));
    if(instruction.find("lose") != std::string_view::npos)
    {
        happiness *= -1;
    }

    return {fromPersonName, toPersonName, happiness};
}
```

During this step, we can also know if this relation mean an increase or a decrease of happiness.

So now, we have all our data ! \o/

All we need to do it, is store them in some **Relationship** object. Moreover, we can also find the list of attendees by storing their names like:

```c++
std::vector<Person> persons;
foreachLineIn(fileContent, [&persons](const std::string& line)
{
    const auto elements = extractInformationFrom (line);
    persons.emplace_back(std::get<0>(elements));
});
auto last = std::unique(persons.begin(), persons.end());
persons.erase(last, persons.end()); 
```

So now that we have all our data, we can go to the algorithm part !

#### The Algorithm

First of all, let's describe what this algorithm must do.
It must be able to find all the combination of attendees around the table, calculate the total amount of happiness on a table, and find the optimal happiness.

There may be other solutions to find all the possible combination of attendees around the table, but we will go with the recursion ! 😱
And it looks like that :

```c++
using Happiness = int;
using PersonsRefs = std::vector<std::reference_wrapper<Person>>;
using Relationships = std::vector<Relationship>;

Happiness gerateTablePosition (PersonsRefs& personsPlaced, PersonsRefs& personsToPlace, const Relationships& relationships)
{
    if(personsToPlace.empty())
    {
        return calculateHappiness(personsPlaced, relationships);
    }

    Happiness optimalHappiness{0};

    for(const auto& personToPlace : personsToPlace)
    {
        auto newPersonsPlaced = personsPlaced;
        auto newPersonsToPlace = personsToPlace;

        newPersonsPlaced.emplace_back(personToPlace);
        newPersonsToPlace.erase(
            std::remove_if(
                std::begin(newPersonsToPlace),
                std::end(newPersonsToPlace),
                [&personToPlace](const auto& person)
                {
                    return personToPlace.get() == person.get();
                }),
            std::end(newPersonsToPlace));

        const auto optimalHappinessSoFar = gerateTablePosition (newPersonsPlaced, newPersonsToPlace, relationships);
        optimalHappiness = std::max(optimalHappiness, optimalHappinessSoFar);
    }
    return optimalHappiness;
}
```

Don't worry, I am going to explain that chuck of code 😉

This function takes several parameters : the list of persons we have already placed, the list of person we yet have to place, and the list of relationships (only necessary to calculate the amount of happiness).
And there is more ! The vector containing the list of persons we have already placed, also represents the places of the person on the table ! 🙊

Indeed, each person placed in this vector has as neighbors the persons placed before and after them, at the index minus one, and plus one. And for the persons placed at the edges of the vector, we loop back of the other edge.

Note that I could have used a [Ring Buffer](https://en.wikipedia.org/wiki/Circular_buffer) to represent this instead of a vector, but I have only thought about it now. I'll probably update it later.

Now, let's go through this algorithm.

First, we check if we have placed everybody. If this is the case, it's good ! We can calculate the amount of happiness of the table.

If this is not the case, then, we have to place the other persons.
To do that, *for each* persons we haven't placed on the table, we created a new table with them in it and rerun the function again. So if it's the last person to place, the next call will calculate the happiness of the table, if not, it will place every other person left and run this function again until the table is full.
And *for each* person placed at the table, we check if the amount of happiness found if the greatest we had so far, and store it, if so.

Once we have finish iterating, we return the optimal amount of happiness found.

We have two other thing to do : call the recursive function, and calculate the amount of happiness of a table.

```c++
Happiness getMostOptimalHappiness(std::vector<Person>& persons, const std::vector<Relationship>& relationships)
{
    std::vector<std::reference_wrapper<Person>> personsToPlaceRefs, personsPlacedRefs;
    std::copy(persons.begin(), persons.end(), std::back_inserter(personsToPlaceRefs));
    personsToPlaceRefs.pop_back();
    personsPlacedRefs.emplace_back(persons.back());
    return gerateTablePosition (personsPlacedRefs, personsToPlaceRefs, relationships);
}
```

Here is the function calling the recursive function. All it does, is to create the persons to place and place the first person on the table.

So let's go to the most interesting part, the calculation of the happiness amount:

```c++
using Happiness = int;
using PersonsRefs = std::vector<std::reference_wrapper<Person>>;
Happiness calculateHappiness(const PersonsRefs& personsPlaced, const std::vector<Relationship>& relationships)
{
    Happiness happiness{0};
    for(auto index = size_t{0}; index < personsPlaced.size(); ++index)
    {
        const auto neighborLeftIndex = (index == 0) ? (personsPlaced.size() - 1) : (index - 1);
        const auto neighborRightIndex = (index == (personsPlaced.size() - 1)) ? 0 : (index + 1);

        const auto leftRelationShip = std::find_if(std::begin(relationships), std::end(relationships),
        [&personsPlaced, neighborLeftIndex, index](const auto& relationship)
        {
            return relationship.isRelationBetween(personsPlaced[index].get(), personsPlaced[neighborLeftIndex].get());
        });
        happiness += leftRelationShip->getHappiness();

        const auto rightRelationShip = std::find_if(std::begin(relationships), std::end(relationships),
        [&personsPlaced, neighborRightIndex, index](const auto& relationship)
        {
            return relationship.isRelationBetween(personsPlaced[index], personsPlaced[neighborRightIndex]);
        });
        happiness += rightRelationShip->getHappiness();
    }
    return happiness;
}
```

Now, all the persons have a sit at the table, we can calculate the amount of happiness of the table.
To do so, we calculate the amount of happiness for each person. And the amount of happiness of each person is described by their relation to their neighbors which is at an index 1 on either side. We only have to be carefully for the persons at the edge of the vector, to make sure we don't go out of the bounds of the vector.

And voilà ! We have an algorithm able to find the optimal amount of happiness on any circular table 😄

## Part 2

### Problem

We forget to add yourself in the seating arrangement 🙀

So we need to do it again, with yourself at the table, knowing that we have add 0 amount of happiness no matter were we are seated at the table.

### Solution

There is several solution for this problem, and I went with the laziest one, by not modifying the code, but only the input.
Indeed, by adding a person named Myself in the input file and a relation to each other persons at the table, I only add to run the program again to find the solution. This was a quick solution since there was not a lot of person. If that wasn't the case, I would have modify the code to add those relationships in the code itself.

## Conclusion

You can note that the solutions, written in this post, don't include all the sources to make running programs, but only the interesting part of the sources to solve this problem.
If you want to see the programs from end to end, you can go on my [GitHub account](https://github.com/Xav83/AdventOfCode/tree/2015.13/2015/Day13), explore the full solution, add comments or ask questions if you want to, on the platform you read this article, it will also help me improve the quality of my articles.

Here is the list of elements that we have used, I can't encourage you enough to look at their definitions :

- [std::regex](https://en.cppreference.com/w/cpp/regex)
- [std::string](https://en.cppreference.com/w/cpp/string/basic_string)
- [std::string_view](https://en.cppreference.com/w/cpp/string/basic_string_view)
- [std::tuple](https://en.cppreference.com/w/cpp/utility/tuple)
- [std::get](https://en.cppreference.com/w/cpp/utility/tuple/get)
- [std::begin](https://en.cppreference.com/w/cpp/iterator/begin)
- [std::end](https://en.cppreference.com/w/cpp/iterator/end)
- [std::vector](https://en.cppreference.com/w/cpp/container/vector)
- [std::unique](https://en.cppreference.com/w/cpp/algorithm/unique)
- [std::copy](https://en.cppreference.com/w/cpp/algorithm/copy)
- [std::reference_wrapper](https://en.cppreference.com/w/cpp/utility/functional/reference_wrapper)
- [std::max](https://en.cppreference.com/w/cpp/algorithm/max)
- [std::find_if](https://fr.cppreference.com/w/cpp/algorithm/find)

Thanks for you reading, hope you liked it 😃

And until next part, have fun learning and growing.
