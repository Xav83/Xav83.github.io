# The Modern Cpp Challenge on Mobile - Abundant numbers

Hello ! I'm Xavier Jouvenot and here is the sixth part of a long series on [The Modern C++ Challenge](https://amzn.to/2QdYmvA).
In this article, I am going to explain how I solved the sixth problem in C++, and how I integrated the solution in an Android project.

The objective of this sixth problem is simple.
We must that prints all all abundant numbers and their abundance, up to a limit entered by the user.
The solution will be computed in C++ and the interface to get the user input and display the result will be handled with the Android Studio Framework.

I encourage you to read the [previous part of this series](https://10xlearner.com/2020/04/20/the-modern-cpp-challenge-on-mobile-sexy-prime-pairs/), since we are going to continue our program created in it.

## What is a abundant number ?

> An abundant number is a number for which the sum of its proper divisors is greater than the number itself. The amount by which the sum exceeds the number is the abundance.

For example, the number `12` can be divided by `1`, `2`, `3`, `4`, `6` and itself.
So its *proper divisors* are `1`, `2`, `3`, `4`, `6`, and if you sum them, you get `1+2+3+4+6 = 16` which is greater than 12.
So `12` is a abundant number with an abundance of `16-12 = 4`.

## The C++ solution

First of all we have to be able to solve the problem using C++.
To do so, we are going to implement 3 functions. 🙂

The first one allowing us to get the sum of all the divisor of a number.

```cpp
static constexpr unsigned int getSumOfDivisors(unsigned int number)
{
    unsigned int sumOfDivisors{0};
    for(auto i = 1; i < number; ++i)
    {
        if(number % i == 0)
        {
            sumOfDivisors += i;
        }
    }
    return sumOfDivisors;
}
```

The second function allows us to know if a number is an abundant number, and to get the abundance if it is an abundant number.
This function will return a [std::optional](https://en.cppreference.com/w/cpp/utility/optional) which will contain the value of the abundance, or [std::nullopt](https://en.cppreference.com/w/cpp/utility/optional/nullopt) if the input is not an abundant number.

```cpp
std::optional<unsigned int> getAbundance(unsigned int number)
{
    const auto sum = getSumOfDivisors(number);
    if(number < sum)
    {
        return {static_cast<unsigned int>(sum - number)};
    }
    return std::nullopt;
}
```

The third and last function allows us to get all the abundant number with their abundance up to the limit given in parameters.

```cpp
std::vector<std::pair<unsigned int, unsigned int>> getAllAbundantNumbersUpTo(unsigned int upperLimit)
{
    std::vector<std::pair<unsigned int, unsigned int>> results;
    for(auto i = 1; i <= upperLimit; ++i)
    {
        auto potentialAbundantNumber = i;
        auto abundance = getAbundance(i);
        if(abundance.has_value())
        {
            results.emplace_back(std::make_pair<unsigned int, unsigned int>(std::move(potentialAbundantNumber), std::move(*abundance)));
        }
    }
        return results;
}
```

With those three functions, we can solve the problem without any problem 😉

## The UI interface on Android Studio

The user interface for this problem is like the one of of the [previous problem](https://10xlearner.com/2020/04/20/the-modern-cpp-challenge-on-mobile-sexy-prime-pairs/). So I won't go into more detail, but I encourage you to go look to my [blog post about it](https://10xlearner.com/2020/04/20/the-modern-cpp-challenge-on-mobile-sexy-prime-pairs/). 😉

But even if the interface for the problem is similar to the previous two problems, I want to talk to you about the user interface I had to put in place.
Indeed, now that we have 6 problems solved in our application, having to click a lot of times on the next button to reach the last problem, is long, and is going to be longer each time that we add a new problem solution.

To avoid that, we have created a user interface similar to a menu where you can click on the problem you want to go to.
To do so, we have created a new user interface with 6 buttons opening one problem interface.

Here are the methods I used:

```java
void addButton(@IdRes int id, String text)
{
    RelativeLayout ll = findViewById(R.id.buttons);

    RelativeLayout.LayoutParams newParams = new RelativeLayout.LayoutParams(
            RelativeLayout.LayoutParams.WRAP_CONTENT,
            RelativeLayout.LayoutParams.WRAP_CONTENT);

    Button b = new Button(this);
    b.setId(id);
    b.setText(text);
    b.setOnClickListener(this);

    newParams.addRule (RelativeLayout.ALIGN_LEFT);
    b.setLayoutParams(newParams);
    ll.addView(b);
}

void addButton(@IdRes int id, String text, @IdRes int idOfLeftElement)
{
    RelativeLayout ll = findViewById(R.id.buttons);

    RelativeLayout.LayoutParams newParams = new RelativeLayout.LayoutParams(
            RelativeLayout.LayoutParams.WRAP_CONTENT,
            RelativeLayout.LayoutParams.WRAP_CONTENT);

    Button b = new Button(this);
    b.setId(id);
    b.setText(text);
    b.setOnClickListener(this);

    newParams.addRule (RelativeLayout.RIGHT_OF, idOfLeftElement);
    b.setLayoutParams(newParams);
    ll.addView(b);
}

void addButtonBelow(@IdRes int id, String text, @IdRes int idOfElementOnTop)
{
    RelativeLayout ll = findViewById(R.id.buttons);

    RelativeLayout.LayoutParams newParams = new RelativeLayout.LayoutParams(
            RelativeLayout.LayoutParams.WRAP_CONTENT,
            RelativeLayout.LayoutParams.WRAP_CONTENT);

    Button b = new Button(this);
    b.setId(id);
    b.setText(text);
    b.setOnClickListener(this);

    newParams.addRule (RelativeLayout.BELOW, idOfElementOnTop);
    b.setLayoutParams(newParams);
    ll.addView(b);
}

void addButtonBelow(@IdRes int id, String text, @IdRes int idOfLeftElement, @IdRes int idOfElementOnTop)
{
    RelativeLayout ll = findViewById(R.id.buttons);

    RelativeLayout.LayoutParams newParams = new RelativeLayout.LayoutParams(
            RelativeLayout.LayoutParams.WRAP_CONTENT,
            RelativeLayout.LayoutParams.WRAP_CONTENT);

    Button b = new Button(this);
    b.setId(id);
    b.setText(text);
    b.setOnClickListener(this);

    newParams.addRule (RelativeLayout.RIGHT_OF, idOfLeftElement);
    newParams.addRule (RelativeLayout.BELOW, idOfElementOnTop);
    b.setLayoutParams(newParams);
    ll.addView(b);
}

@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);

    addButton(R.id.menu_button_1, "1");
    addButton(R.id.menu_button_2, "2", R.id.menu_button_1);
    addButton(R.id.menu_button_3, "3", R.id.menu_button_2);
    addButton(R.id.menu_button_4, "4", R.id.menu_button_3);
    addButtonBelow(R.id.menu_button_5, "5", R.id.menu_button_1);
    addButtonBelow(R.id.menu_button_6, "6", R.id.menu_button_5, R.id.menu_button_2);
}

public void onClick(View v) {
    switch (v.getId())
    {
        case R.id.menu_button_1:
            startActivity(new Intent(this, Problem_1.class));
            break;
        case R.id.menu_button_2:
            startActivity(new Intent(this, Problem_2.class));
            break;
        case R.id.menu_button_3:
            startActivity(new Intent(this, Problem_3.class));
            break;
        case R.id.menu_button_4:
            startActivity(new Intent(this, Problem_4.class));
            break;
        case R.id.menu_button_5:
            startActivity(new Intent(this, Problem_5.class));
            break;
        case R.id.menu_button_6:
            startActivity(new Intent(this, Problem_6.class));
            break;
        default:
            new AssertionError("Unknown menu button.");
            break;
    }
}
```

Don't worry, I am going to explain this code. 😉

First of all, the `addButton` and `addButtonBelow` methods create the button on the user interface with the right text and in the right position, by using a `RelativeLayout` placed on the user interface in the xml definition of the user interface.

Then, we have the `onCreate` method which calls the methods previously defined to add the buttons with the right parameters.

And finally, the method `onClick` is called when a button is clicked on and open the right activity depending on the button clicked. This implies that the class implements the class `View.OnClickListener`. 😉

And like that, we end up with a menu allowing us to go directly to the problem we want.

## Using C++ native code

The linking the user interface to the C++ code is very similar to the previous problem.

```cpp
extern "C"
JNIEXPORT jstring JNICALL
Java_com_example_themoderncppchallenge_Problem_16_AbundantNumbersUpTo(JNIEnv *env, jobject thiz,
                                                                      jint user_input) {
    const auto& result = getAllAbundantNumbersUpTo(user_input);
    std::string text;
    for(const auto& abundance : result) {
        text += '(' + std::to_string(abundance.first) + ", " + std::to_string(abundance.second) + "), ";
    }
    // Removes the last ", "
    text.pop_back();
    text.pop_back();
    return env->NewStringUTF(text.c_str());
}
```

We start by getting the results from the previously defined function, and we transform those data in a `string` before sending it to the caller.

## Conclusion

Voilà ! We now have an application which can solve the first six problems of [The Modern C++ Challenge](https://amzn.to/2QdYmvA).

You can note that the solutions, written in this post, don't include all the sources to make a running program, but only the interesting part of the sources to solve this problem.
If you want to see the programs from end to end, you can go on my [GitHub account](https://github.com/Xav83/TheModernCppChallenge_AndroidStudio/tree/v0.0.6_SixthProblem), explore the full solution, add comments or ask questions if you want to, on the platform you read this article, it will also help me improve the quality of my articles.

Thank you all for reading this article,
And until my next article, have a splendid day 😉

![](https://github.com/Xav83/Xav83.github.io/raw/master/res/TheModernCppChallenge/Problem%20006.png "Final Application")
![](https://github.com/Xav83/Xav83.github.io/raw/master/res/TheModernCppChallenge/Problem%20006%20-%20Menu.png "Menu")

## Interesting links

- [The Modern C++ Challenge](https://amzn.to/2QdYmvA)
- [Github release](https://github.com/Xav83/TheModernCppChallenge_AndroidStudio/tree/v0.0.6_SixthProblem)
- [Abundant number definition](https://en.wikipedia.org/wiki/Abundant_number)
- [Button documentation](https://developer.android.com/reference/android/widget/Button)
- [RelativeLayout documentation](https://developer.android.com/guide/topics/ui/layout/relative)
- [std::optional documentation](https://en.cppreference.com/w/cpp/utility/optional)
