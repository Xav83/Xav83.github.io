# Advent Of Code - All in a Single Night - Puzzle 9

Hello ! I'm Xavier Jouvenot and here is the nineth part of a long series on [Advent Of Code](https://adventofcode.com). You can find the previous part [here](2019-08-19-AdventOfCode2015-Day8-Matchsticks.md)

For this new post, we are going to solve the problem from the 9th December 2015, named "All in a Single Night".
The solution I will propose in C++, but the reasoning can be applied to other languages.

## Part 1

### The Problem

The full version of this problem can be found directly on the [Advent of Code website](https://adventofcode.com/2015/day/9), I will only describe the essence of the problem here:

This year, Santa has some new locations to visit to be able deliver all of his presents in a single night.
His elves have provided him the distances between every pair of locations, and we need with their information to found the shortest distance he can travel to achieve this, passing exactly one time in each city?

For example, given the following distances:

```
London to Dublin = 464
London to Belfast = 518
Dublin to Belfast = 141
```

The shortest of these is `London -> Dublin -> Belfast = 605`, and so the answer is `605`.

### Solution

So, if we remove all the Santa vocabulary from this problem, we will have to find the shortest path in a graph going through each node only once. But first of all, we need to extract the information to build the graph !

To extract the cities, I've based myself on the position of the "to" world and on the "=", and ended up with a code like that:
```c++
std::pair<City, City> getCitiesFromInstruction (const std::string_view instruction)
{
    const auto citySeparatorPosition = instruction.find(" to ");
    const auto equalSeparatorPosition = instruction.substr(citySeparatorPosition+4).find(" = ");
    return std::make_pair(City(instruction.substr(0, citySeparatorPosition)), City(instruction.substr(citySeparatorPosition+4, equalSeparatorPosition)));
}
```

whereas for the distance between the I used a regex, because why not 😛

```c++
Distance getDistanceFromInstruction (const std::string& instruction)
{
    std::regex word_regex("[0-9]+");
    auto words_begin = std::sregex_iterator(std::begin(instruction), std::end(instruction), word_regex);
    auto words_end = std::sregex_iterator();

    auto value{0};

    for (std::sregex_iterator i = words_begin; i != words_end; ++i)
    {
        std::smatch match = *i;
        return static_cast<Distance>(atoi(match.str().c_str()));
    }
    assert (false);
    return 0;
}
```

Now that we have our data, we can store them in the graph before searching for the shortest path. I won't detail what classes I created, I've basically made one class for a `City`, one for a `Distance`, one for a `Route`, and one for a `Graph`.

Let's jump directly to the main algorithm !

It consists of two functions, a function `run` which will create the elements we will need in the second function, like a sorted graph and and some storage for the cities we have visited and the cities we still have to visit, before calling the second function. This function look like this:

```c++
Distance run()
{
    std::vector<Node> citiesToVisit, citiesVisited;
    std::sort(std::begin(sortedGraph), std::end(sortedGraph));
    citiesToVisit = sortedGraph;
    return travel (citiesToVisit, 0, citiesVisited);
}
```

As you can see, for now, the cities we have to visit consists of the sorted graph and and the cities visited is empty, since we haven't visited any city yet. And the second function, as you may have noticed is named `travel` and takes three arguments : the cities we still have to visit, the distance traveled until now and the cities we have visited for now. This function is a [recursive function](https://en.wikipedia.org/wiki/Recursion_(computer_science)), in which, we are going to look at each road passing through all the cities only once, and compute the distance traveled, and compare to the shorted distances found, until we figure out the shortest path of the graph.

Enough talking, let's look at the code !

```
Distance travel(const std::vector<Node>& citiesToVisit, Distance distanceTraveledUntilNow, const std::vector<Node>& citiesVisited)
{
    if(citiesToVisit.empty())
    {
        return getRouteDistance();
    }
    auto min = std::numeric_limits<Distance>::max();
    for(const auto& city : citiesToVisit)
    {
        std::vector<Node> citiesVisitedWithNewCity, citiesVisitedWithNewCitySorted, lastCitiesToVisit;

        citiesVisitedWithNewCity = citiesVisited;
        citiesVisitedWithNewCity.emplace_back(city);

        citiesVisitedWithNewCitySorted = citiesVisitedWithNewCity;
        std::sort(std::begin(citiesVisitedWithNewCitySorted), std::end(citiesVisitedWithNewCitySorted));

        std::set_difference(std::begin(sortedGraph), std::end(sortedGraph), std::begin(citiesVisitedWithNewCitySorted), std::end(citiesVisitedWithNewCitySorted), std::back_inserter(lastCitiesToVisit));

        const auto distanceTraveled = travel (lastCitiesToVisit, distanceTraveledUntilNow, citiesVisitedWithNewCity);
        min = std::min(min, distanceTraveled);
    }

    return distanceTraveledUntilNow + min + getRouteDistance();
}
```

Wow, it's a pretty big piece of code. Let's explain it little by little.
First, we have :

```c++
if(citiesToVisit.empty())
{
    return getRouteDistance();
}
```

This is the `stopping criterion` of the `travel` method. This condition allow the recursive method to stop. Indeed, when we no longer have cities to visit, we can return the route's distance of the two last visited cities with the method `getRouteDistance`.

Then, we have the for loop :

```c++
for(const auto& city : citiesToVisit)
{
    std::vector<Node> citiesVisitedWithNewCity, citiesVisitedWithNewCitySorted, lastCitiesToVisit;

    citiesVisitedWithNewCity = citiesVisited;
    citiesVisitedWithNewCity.emplace_back(city);

    citiesVisitedWithNewCitySorted = citiesVisitedWithNewCity;
    std::sort(std::begin(citiesVisitedWithNewCitySorted), std::end(citiesVisitedWithNewCitySorted));

    std::set_difference(std::begin(sortedGraph), std::end(sortedGraph), std::begin(citiesVisitedWithNewCitySorted), std::end(citiesVisitedWithNewCitySorted), std::back_inserter(lastCitiesToVisit));

    const auto distanceTraveled = travel (lastCitiesToVisit, distanceTraveledUntilNow, citiesVisitedWithNewCity);
    // ....
}
```

All these container's manipulations are the main part of the recursion! It allows us to call the method `travel` to call itself with one city traveled added to the container of the cities already traveled and to reduce the number of cities yet to visit.
Moreover, the for loop makes it such as, we call the `travel` method with different ensembles of cities traveled and to traveled to, since we add a different city from the cities to travel into the cities traveled (I let you read that several times since it's that not easy to get if you have no bases on recursive methods).

Now because of this recursive method, we will be able to `travel` through all the paths of the graph that passes only once by each city. All we have now to do, is to find the shortest one. And this is what the last part of the function `travel` is for :

```c++
auto min = std::numeric_limits<Distance>::max();
for(const auto& city : citiesToVisit)
{
    // ...
    const auto distanceTraveled = travel (lastCitiesToVisit, distanceTraveledUntilNow, citiesVisitedWithNewCity);
    min = std::min(min, distanceTraveled);
}

return distanceTraveledUntilNow + min + getRouteDistance();
```

So each time a travel function returns a distance (either because it reached the `stopping criterion` or because of the code above), we check if this distance is the shortest among all the `travel` calls into the for loop. Once the for loop ends, we have the shortest distance of the paths followed by the `travel` method for this point in the algorithm, and we can return the distance traveled which is the sum of the distance traveled until now (a parameter of the `travel` function), the shorted path from the `travel` methods call in the for loop, and the distance between the two last cities visited.

And here it is we have the complete algorithm. The explication is pretty complex, and not so easy to get, so I can only encourage you to try it by yourself to modify it to see what is happening. You can code it yourself or use my code as base to look at it on my [GitHub](https://github.com/Xav83/AdventOfCode/tree/2015.09/2015/Day9)

And now, let's see what the second part have for us 😃

## Part 2

### The Problem

Why take the shortest path when you can take the longest path ? Isn't the journey what more important that the destination ? 😉

Well the problem isn't describe as such, but we need now to find the longest distance passing through each city exactly once, because Santa wants it.

### Solution

Everything is exactly the same except for one line ! Do you know which one ?

... (jeopardy music)

Such suspense (or maybe not 😆), so the line to change is the one calculating the min.
Indeed, changing this one by a call to a `std::max`, instead of a `std::min` does the job perfectly.
I'm glad it was that easy, since the first part wasn't easy at all!

And if we want to have our variable correctly name, we end up with the last part of the code looking like:

```c++
auto max = std::numeric_limits<Distance>::max();
for(const auto& city : citiesToVisit)
{
    // ...
    const auto distanceTraveled = travel (lastCitiesToVisit, distanceTraveledUntilNow, citiesVisitedWithNewCity);
    max = std::max(max, distanceTraveled);
}

return distanceTraveledUntilNow + max + getRouteDistance();
```

And you have it, the solution of the second part 😄

## Conclusion

You can note that the solutions, written in this post, don't include all the sources to make running programs, but only the interesting part of the sources to solve this problem.
If you want to see the programs from end to end, you can go on my [GitHub account](https://github.com/Xav83/AdventOfCode/tree/2015.09/2015/Day9), explore the full solution, add comments or ask questions if you want to, on the platform you read this article, it will also help me improve the quality of my articles.

Here is the list of std method that we have used, I can't encourage you enough to look at their definitions :

- [std::max](https://en.cppreference.com/w/cpp/algorithm/max)
- [std::min](https://en.cppreference.com/w/cpp/algorithm/min)
- [std::numeric_limits](https://en.cppreference.com/w/cpp/types/numeric_limits)
- [std::set_difference](https://en.cppreference.com/w/cpp/algorithm/set_difference)
- [std::vector](https://en.cppreference.com/w/cpp/container/vector)
- [std::begin](https://en.cppreference.com/w/cpp/iterator/begin)
- [std::end](https://en.cppreference.com/w/cpp/iterator/end)
- [std::back_inserter](https://en.cppreference.com/w/cpp/iterator/back_inserter)
- [std::sort](https://en.cppreference.com/w/cpp/algorithm/sort)
- [std::regex](https://en.cppreference.com/w/cpp/regex)
- [std::pair](https://en.cppreference.com/w/cpp/utility/pair)
- [std::string_view](https://en.cppreference.com/w/cpp/string/basic_string_view)

Thanks for you reading, hope you liked it 😃

And until next part, have fun learning and growing.
