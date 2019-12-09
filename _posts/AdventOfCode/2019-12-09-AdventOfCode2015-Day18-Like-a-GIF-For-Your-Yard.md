# Advent Of Code - Like a GIF For Your Yard - Puzzle 18

Hello ! I'm Xavier Jouvenot and here is the part eighteenth of a long series on [Advent Of Code](https://adventofcode.com). You can find the previous part [here](2019-12-03-AdventOfCode2015-Day17-No-Such-Thing-as-Too-Much.md)

For this new post, we are going to solve the problem from the 18th December 2015, named "Like a GIF For Your Yard".
The solution I will propose in C++, but the reasoning can be applied to other languages.

## Part 1

### The Problem

The full version of this problem can be found directly on the [Advent of Code website](https://adventofcode.com/2015/day/18), I will only describe the essence of the problem here:

Once again, we have a grid of lights (100x100), and this time, we want to make beautiful animation on it.
But there are some rules that allows us to go to the next state of the grid, and create a beautiful animation by doing so:
- A light which is **on** stays on when `2` or `3` neighbors are on, and turns off otherwise.
- A light which is **off** turns on if exactly `3` neighbors are on, and stays off otherwise.

And all of the lights update simultaneously; they all consider the same current state before moving to the next.

The objective, is to know, how many lights are **on** after 100 steps?

### Solution

What they demand us here, is an implementation of [Conway's Game of Life](https://en.wikipedia.org/wiki/Conway's_Game_of_Life). I invite you to check it, this is a really interesting zero-player game 🙂

Since it is a very well known game, there are plenty of implementation online, probably way better than the one I will propose to you, but still, it's always a good exercise to figure out a solution by yourself before checking other solutions.

Just by reading the problem text, you can imagine that we are going to need some `Light`, `Grid` objects and to figure out their behavior. So let's take a look at the solution I propose to you, starting with the `Light` 😉

```c++
using LightState = bool;

class Light {
public:
  Light(LightState initialState);
  ~Light();

  bool isOn() const;
  bool isOff() const;

  void
  prepareNextState(const std::vector<std::reference_wrapper<Light>> &neighbors);

  void nextState();

private:
  LightState currentState = false;
  LightState willChangeState = false;
};
```

So here is the interface of this class.
Each light will have an initial state (depending on the initial configuration). We can check if it is on or off. We can also prepare for the next state of this light depending on its neighbors, to figure out if the state of the light will change in the next state. And finally, we can act the decision taken during the preparation.

The methods giving the state of the light are trivial, so we are only going to detail the other two methods:

```c++
void Light::prepareNextState(
    const std::vector<std::reference_wrapper<Light>> &neighbors) {
  const auto numberOfNeighborsOn =
      std::count_if(std::begin(neighbors), std::end(neighbors),
                    [](const auto &light) { return light.get().isOn(); });
  if (isOff() && numberOfNeighborsOn == 3) {
    willChangeState = true;
  } else if (isOn() && (numberOfNeighborsOn != 2 && numberOfNeighborsOn != 3)) {
    willChangeState = true;
  }
}

void Light::nextState() {
  if (willChangeState) {
    currentState = !currentState;
    willChangeState = false;
  }
}
```

During the preparation, we count the number of neighbors that are on, using the [std::count_if](https://en.cppreference.com/w/cpp/algorithm/count) algorithm.
Then, we apply the rules to determine when the state of the light will change. If the light is off, it will change only if it has three neighbors turned on. And if the light is on, it will stays on if it has 2 or 3 neighbors, but, since we want to know when the light will change state, we take the negation of this condition. 😉

And here we have a well functioning light.
Now, let's take a look to the `Grid`.

```c++
class Grid {
public:
  Grid();
  ~Grid();

  size_t numberOfLightsOn() const;

  // Builds the grid
  void emplaceLight(LightState lightState);
  void nextLine();

  // Run the animation
  void nextStep();

private:
  std::vector<std::vector<Light>> lights;
};
```

Here is the interface of the `Grid` of lights with which you can build the grid with any number of light you want, make the animation advance from one state to the other, and get the number of lights turned on. 🙂
First, let's take a look at how we create this `Grid`:

```c++
// Grid methods
void Grid::emplaceLight(LightState lightState) {
  lights.back().emplace_back(lightState);
}
void Grid::nextLine() { lights.emplace_back(); }

// In the main
foreachLineIn(fileContent, [&lights](const std::string &line) {
  lights.nextLine();
  for (const auto &character : line) {
    lights.emplaceLight(character == '#');
  }
});
```

This is how you create your initial configuration of your `Grid` of lights with this interface : for each line of the init configuration file, each light turned on is represented by a `#`, so we create a new line, and add each light with the state given by the condition `character == '#'`. The implementation of the `Grid` is pretty straight forward too, using method of [std::vector](https://en.cppreference.com/w/cpp/container/vector).

Now, let's look at the implementation of the two others methods:

```c++
void Grid::nextStep() {
  // Preparation
  for (size_t i{0}; i < lights.size(); ++i) {
    for (size_t j{0}; j < lights[i].size(); ++j) {
      std::vector<std::reference_wrapper<Light>> neighbors;
      if (i > 0) { neighbors.push_back(lights[i - 1][j]); }
      if (j > 0) { neighbors.push_back(lights[i][j - 1]); }

      if (i < (lights.size() - 1)) { neighbors.push_back(lights[i + 1][j]); }
      if (j < (lights[i].size() - 1)) { neighbors.push_back(lights[i][j + 1]); }

      if (i > 0 && j > 0) { neighbors.push_back(lights[i - 1][j - 1]); }
      if (i < (lights.size() - 1) && j < (lights[i].size() - 1)) { neighbors.push_back(lights[i + 1][j + 1]); }

      if (i > 0 && j < (lights[i].size() - 1)) { neighbors.push_back(lights[i - 1][j + 1]); }
      if (i < (lights.size() - 1) && j > 0) { neighbors.push_back(lights[i + 1][j - 1]); }

      lights[i][j]->prepareNextState(neighbors);
    }
  }
  // Next State
  for (size_t i{0}; i < lights.size(); ++i) {
    for (size_t j{0}; j < lights[i].size(); ++j) {
      lights[i][j].nextState();
    }
  }
}

size_t Grid::numberOfLightsOn() const {
  auto sum = 0;
  for (size_t i{0}; i < lights.size(); ++i) {
    sum += std::count_if(std::begin(lights[i]), std::end(lights[i]),
                         [](const auto &light) { return light.isOn(); });
  }
  return sum;
}
```

First, let's start with the smallest one, the number of lights on. To do that, we iterate on each rows in the grid, and count the number of light turned on on each row. We sum the numbers found and we have our solution.

Now, let's take a look to the much bigger method, the one enabling us to go to the next state of the `Grid`. 🤔
This method could have been split in two, since the first part is all about the preparation of each light, and the second part runs the method `nextState` on each light. The difficulty in this method is to create the neighbors of each light during the preparation step. Depending on where the light is on the Grid (if it is on an edge or not), the number and the position of the neighbors will change, so we have to adapt ourselves to that.

All those condition are pretty costly, and there are solutions to improve that. We could have for example integrated the neighbors into the `Light` class, so that we would only have to figure out the neighbors once for each light.
Another solution, often used in [Conway's Game of Life](https://en.wikipedia.org/wiki/Conway's_Game_of_Life) implementation is to add a layer of lights around the grid, and make those light always turned on, so that the light we are interested in always have eight neighbors. 😃

And to conclude this part of the day, all we need is to run the `Grid::nextStep` function 100 times to found the solution:
```c++
for (auto i = 0; i < 100; ++i) {
  grid.nextStep();
}

const auto numberOfLightsOn = grid.numberOfLightsOn();
```

Et voilà, a beautiful animation for our grid 😉

## Part 2

### Problem

Once the grid set up, we notice that the light on each corner of the grid are **stuck on**, and can't be turned off. This completely the behavior of the animation, so we need to figure out how many lights are **on** after 100 steps with the lights always on.

### Solution

So, there are now some light with some special behavior which sounds like we need some inheritance.
Actually, using inheritance, the behavior of the light in the corner is pretty easy to implement:

```c++
class CornerLight : public Light {
public:
  CornerLight() : Light(true) {}
  ~CornerLight() = default;

  bool isOn() const override { return true; }
  bool isOff() const override { return false; }
};
```

And here we have a `CornerLight` which stays always on (don't forget to set the method as `virtual` in the `Light` class).

So creating the class was easy, but integrate it a bit longer. 🤔
Indeed, we haven't made a `Grid` class able to deal with inheritance on `Light`, so we have to modify it by adding some method:
```c++
class Grid {
public:
// ... (other methods)
  void pushBackLight(std::unique_ptr<Light> light);

  std::vector<std::vector<std::unique_ptr<Light>>> &getLights();

private:
  std::vector<std::vector<std::unique_ptr<Light>>> lights;
```

So we have modified the `Light` by a `std::unique_ptr<Light>` to be able to use some inheritance, and add a method to add `Light` and its inherited classes into the grid. The implementation of this method is simple:
```c++
void Grid::emplaceLight(LightState lightState) {
  lights.back().emplace_back(std::make_unique<Light>(lightState));
}

void Grid::pushBackLight(std::unique_ptr<Light> light) {
  lights.back().emplace_back(std::move(light));
}
```

We also had to adapt `Grid::emplaceLight(LightState lightState)` since we have now a `std::unique_ptr` 🙂

So now, we have a new king of light, we have adapted the `Grid` to be able to deal with it, all we have left to do, is to add the special light at the right place.

```c++
auto &grid = lights.getLights();
const auto &lastLightIndex = grid[0].size() - 1;
grid[0][0] = std::make_unique<CornerLight>();
grid[0][grid[0].size() - 1] = std::make_unique<CornerLight>();
grid[grid.size() - 1][0] = std::make_unique<CornerLight>();
grid[grid.size() - 1][grid[grid.size() - 1].size() - 1] = std::make_unique<CornerLight>();
```

This is one way to do it. After creating the whole grid with simple `Light`, you modify the 4 corners to have `CornerLight` into them, before running the 100 steps 😉

And voilà, you have all the elements interesting on this second part :smile:

## Conclusion

You can note that the solutions, written in this post, don't include all the sources to make running programs, but only the interesting part of the sources to solve this problem.
If you want to see the programs from end to end, you can go on my [GitHub account](https://github.com/Xav83/AdventOfCode/tree/2015.18/2015/Day18), explore the full solution, add comments or ask questions if you want to, on the platform you read this article, it will also help me improve the quality of my articles.

Here is the list of elements that we have used, I can't encourage you enough to look at their definitions :

- [std::vector](https://en.cppreference.com/w/cpp/container/vector)
- [std::begin](https://en.cppreference.com/w/cpp/iterator/begin)
- [std::end](https://en.cppreference.com/w/cpp/iterator/end)
- [std::count_if](https://en.cppreference.com/w/cpp/algorithm/count)
- [std::unique_ptr](https://en.cppreference.com/w/cpp/memory/unique_ptr)
- [std::make_unique](https://en.cppreference.com/w/cpp/memory/unique_ptr/make_unique)
- [std::move](https://en.cppreference.com/w/cpp/utility/move)
- [std::reference_wrapper](https://en.cppreference.com/w/cpp/utility/functional/reference_wrapper)

Thanks for you reading, hope you liked it 😃

And until next part, have fun learning and growing.
