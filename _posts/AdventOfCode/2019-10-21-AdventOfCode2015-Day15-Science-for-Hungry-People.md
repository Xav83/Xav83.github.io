# Advent Of Code - Science for Hungry People - Puzzle 15

Hello ! I'm Xavier Jouvenot and here is the part fifteen of a long series on [Advent Of Code](https://adventofcode.com). You can find the previous part [here](2019-10-14-AdventOfCode2015-Day14-Reindeer-Olympics.md)

For this new post, we are going to solve the problem from the 15th December 2015, named "Science for Hungry People".
The solution I will propose in C++, but the reasoning can be applied to other languages.

## Part 1

### The Problem

The full version of this problem can be found directly on the [Advent of Code website](https://adventofcode.com/2015/day/15), I will only describe the essence of the problem here:

Today, we have to create the perfect milk-dunking cookie recipe. To do so, we have to find the right balance of ingredients.

Our recipe must contain 100 teaspoons of ingredients, and to create our recipe, we know the ingredients we are going to use and their properties per teaspoon : `capacity`, `durability`, `flavor`, `texture` and `calories`. To evaluate if the recipe is good or not, we can calculate a score for the recipe by adding up each of the properties (negative totals become 0) and then multiplying together everything except calories.

For instance, if we have the following ingredients:

```
Butterscotch: capacity -1, durability -2, flavor 6, texture 3, calories 8
Cinnamon: capacity 2, durability 3, flavor -2, texture -1, calories 3
```

The best recipe will be 44 teaspoons of butterscotch and 56 teaspoons of cinnamon, and the score of this recipe will look like this : `total_capacity * total_durability * total_flavor * total_texture`, and each total property is calculated with the property of each ingredient multiplied by the number of teaspoon of this ingredient. For example, the total_capacity calculation is : `total_capacity = -1 * 44 + 2 * 56 = 68`

To have our perfect recipe, we have to find the one with the best score.

### Solution

The perfect recipe for cookies, I'm hungry just thinking about it ! 🍪

First of all, we must extract the important information, all the ingredients properties, from the input.

```c++
constexpr auto numberOfProperties = 4;
using PropertyValue = int;
using Ingredient = std::array<PropertyValue, numberOfProperties>;

Ingredient extractIngredientFrom (const std::string& line)
{
    std::istringstream stream(line);
    std::string ingredientName, capacity, comma, durability, flavor, texture;
    PropertyValue capacityValue, durabilityValue, flavorValue, textureValue;
 
    stream >> ingredientName >> capacity >> capacityValue >> comma >> durability >> durabilityValue >> comma >> flavor >> flavorValue >> comma >> texture >> textureValue;
    return Ingredient{capacityValue, durabilityValue, flavorValue, textureValue};
}
```

All the information we need are the properties of the ingredients.
We don't even need the calories are the name of the ingredient.

Moreover, you can see that, in the instruction collecting the information from `stream`, we have used the variable comma several times.
We actually could have only used one garbage variable for all the elements we did not wanted to get.
But I choose not to, to make the instruction reflect the structure of the input, and make it easier to read.

So now, we have to the ingredients and their properties that we can use for the recipe.
We have from this ingredients properties, we must find the proportion to of those ingredients to get the best recipe.
To achieve this, we are going to start by generate all the proportion possible depending on the number of ingredients.

```c++
std::vector<Proportions> createAllProportionPossibles(size_t numberOfIngredients, size_t teaspoonNumber)
{
    if (numberOfIngredients == 1)
    {
        return {Proportions{teaspoonNumber}};
    }
    std::vector<Proportions> proportions;
    for(auto i = size_t{1}; i < teaspoonNumber; ++i)
    {
        const auto proportionsOfLessIngredients = createAllProportionPossibles (numberOfIngredients - 1, teaspoonNumber - i);
        for(const auto& proportionsOfLessIngredient : proportionsOfLessIngredients)
        {
            Proportions p {i};
            p.insert(p.end(), proportionsOfLessIngredient.begin(), proportionsOfLessIngredient.end());
            proportions.push_back(p);
        }
    }
    return proportions;
}
```

Easy, right ?... Just kidding, we actually are going to explain this code 😉

First thing to know is that we are using recursion. Why ? Because let's say you have two ingredients, for 3 teaspoons.
The proportions possible would be a list of a pair of numbers (1, 2), and (2, 1).
Now, let's take three ingredients for 4 teaspoons, so the proportion will be (1, **1, 2**), (1, **2, 1**) and (2, **1, 1**).
As you can see, the bold number in the first triplets are the one from the list of proportions for two ingredients, for 3 teaspoons. And in the last pair, the two bold number is the only proportion possible for 2 ingredients and 2 teaspoons, since the 2 others are taken by the first ingredient.

Let's dive in the code, it will make it clearer.

So first we have the stop condition of the recursion.
Indeed, if we have only one ingredient, we give it all the teaspoon we have.

Now, the recursion.
In this part of the code, we give the first ingredient more and more teaspoon with the first loop.
Then, we generate all the proportions possible without this ingredient and the teaspoon given to it.
Finally, with the last loop, we generate the proportions by appending the number of teaspoon of the first ingredient to all the proportion generated without it.

And then we have it, we can generate all the proportion whatever the number of teaspoon and ingredients.

All we have left to do is to find which proportion is the best one.

```c++
constexpr auto teaspoonsNumber = 100;
const auto allProportionsPossible = createAllProportionPossibles (ingredients.size(), teaspoonsNumber);

const auto winningProportion = std::max_element(
    std::begin(allProportionsPossible),
    std::end(allProportionsPossible),
    [&ingredients](const auto& firstProportion, const auto& secondProportion)
    {
        return calculateTotalScore(ingredients, firstProportion) < calculateTotalScore(ingredients, secondProportion);
    });

const auto bestTotalScore = calculateTotalScore(ingredients, *winningProportion);
```

As you can see, after generating all the proportion possible, we use the `std::max_element` algorithm to find the best one. We are also using a method named `calculateTotalScore` which calculate the score of one proportion.
So let's look at its body.

```c++
int calculateTotalScore(std::vector<Ingredient> ingredients, Proportions proportion)
{
    auto score{1};
    for(auto propertyIndex = size_t{0}; propertyIndex < numberOfProperties; ++propertyIndex)
    {
        auto propertyScore{0};
        for(auto indexIngredient = size_t{0}; indexIngredient < ingredients.size(); ++indexIngredient)
        {
            const auto& ingredient = ingredients[indexIngredient];
            propertyScore += ingredient[propertyIndex] * proportion[indexIngredient];
        }
        if(propertyScore < 0)
        {
            score = 0;
            break;
        }
        score *= propertyScore;
    }
    return score;
}
```

As described by the text of the problem, we calculate each the property score by multiplying the ingredient property by the proportion of this ingredient, and then, we multiply the found property score to the total score.
Once all the properties done, we have the result.

And voilà, with that last bit of code, we are able to found the best recipe of the cookies 🥇

## Part 2

### Problem

Great, now our recipe is popular and we want to make the best recipe for a `500` calories cookie.

### Solution

The logic to resolve this part is the same as the one used in the first part.
First we collect the information, then we generate all the possible proportions and finally we found the winning proportion of ingredient.
The generation of the proportions stays the same as in the first part. But the information collection, now we collect the number of calories which was useless before. So that it looks like that:

```c++
constexpr auto numberOfProperties = 5;
using PropertyValue = int;
using Ingredient = std::array<PropertyValue, numberOfProperties>;

Ingredient extractIngredientFrom (const std::string& line)
{
    std::istringstream stream(line);
    std::string ingredientName, capacity, comma, durability, flavor, texture, calories;
    PropertyValue capacityValue, durabilityValue, flavorValue, textureValue, caloriesValue;
 
    stream >> ingredientName >> capacity >> capacityValue >> comma >> durability >> durabilityValue >> comma >> flavor >> flavorValue >> comma >> texture >> textureValue >> comma >> calories >> caloriesValue;
    return Ingredient{capacityValue, durabilityValue, flavorValue, textureValue, caloriesValue};
}
```

And finally, the most change part is the calculation of the score of a recipe.

```c++
int calculateTotalScore(std::vector<Ingredient> ingredients, Proportions proportion)
{
    auto score{1};
    for(auto propertyIndex = size_t{0}; propertyIndex < numberOfProperties; ++propertyIndex)
    {
        auto propertyScore{0};
        for(auto indexIngredient = size_t{0}; indexIngredient < ingredients.size(); ++indexIngredient)
        {
            const auto& ingredient = ingredients[indexIngredient];
            propertyScore += ingredient[propertyIndex] * proportion[indexIngredient];
        }
        if(propertyIndex == numberOfProperties - 1)
        {
            return (propertyScore != 500) ? 0 : score;
        }
        if(propertyScore < 0)
        {
            return 0;
        }
        score *= propertyScore;
    }
    return score;
}
```

Most of the calculation stays the same.
We still calculate each property score, but if this is the last property, aka the calory property, we check if it's score is equal to `500`. If this is not the case, we set the score to `0` else, we integrate it to the final score.

And like that, we have a solution to the second part of this problem 😃

## Conclusion

You can note that the solutions, written in this post, don't include all the sources to make running programs, but only the interesting part of the sources to solve this problem.
If you want to see the programs from end to end, you can go on my [GitHub account](https://github.com/Xav83/AdventOfCode/tree/2015.15/2015/Day15), explore the full solution, add comments or ask questions if you want to, on the platform you read this article, it will also help me improve the quality of my articles.

Here is the list of elements that we have used, I can't encourage you enough to look at their definitions :

- [std::max_element](https://en.cppreference.com/w/cpp/regex)
- [std::begin](https://en.cppreference.com/w/cpp/iterator/begin)
- [std::end](https://en.cppreference.com/w/cpp/iterator/end)
- [std::istringstream](https://en.cppreference.com/w/cpp/io/basic_istringstream)
- [std::string](https://en.cppreference.com/w/cpp/string/basic_string)
- [using](https://en.cppreference.com/w/cpp/language/type_alias)
- [std::array](https://en.cppreference.com/w/cpp/container/array)
- [std::vector](https://en.cppreference.com/w/cpp/container/vector)
- [constexpr](https://en.cppreference.com/w/cpp/language/constexpr)

Thanks for you reading, hope you liked it 😃

And until next part, have fun learning and growing.
