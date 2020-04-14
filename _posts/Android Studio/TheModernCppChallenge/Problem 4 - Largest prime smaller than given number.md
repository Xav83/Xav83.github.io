# The Modern Cpp Challenge on Mobile - Largest prime smaller than given number

Hello ! I'm Xavier Jouvenot and here is the fourth part of a long series on [The Modern C++ Challenge](https://amzn.to/2QdYmvA).
In this article, I am going to explain how I solved the fourth problem in C++, and how I integrated the solution in an Android project.

The objective of this third problem is simple.
We must calculate the largest prime number that is smaller than a number provided by the user, and then, we print it to the user.
The solution will be computed in C++ and the interface to get the user input and display the result will be handled with the Android Studio Framework.

I encourage you to read the [previous part of this series](https://10xlearner.com/2020/04/08/the-modern-cpp-challenge-on-mobile-least-common-multiplier/), since we are going to continue our program created in it.

## The C++ solution

To get the largest prime number that is smaller then a number provided by the user, we first must be able to recognize what a prime number is. 😉
A [prime number](https://en.wikipedia.org/wiki/Prime_number) is a natural number greater than 1 that can be only divided by 2 numbers: 1 and itself.

A function implementing such definition looks like that:

```C++
static constexpr bool isPrime(unsigned int number)
{
    if(number < 2) { return false; }

    for(auto divisor = (number / 2); divisor > 1; --divisor)
    {
        if(number % divisor == 0)
        {
            return false;
        }
    }
    return true;
}
```

The first condition validate that the number must be greater than 1 to be prime.
Then, we iterate from half the user input, because we deal with natural number, so no need to check the numbers greater than the half of the user input since that any of those number multiplied by at least 2, will be greater than our number, saving ourselves some computation time. We iterate down to 2 but stop if one of the number can divide the user input. In that case, we return `false`, since the user input can be divided by a number different from 1 and itself, so the user input is not prime. But if, we can find a number matching this condition, we return `true`, as we have find out that this user input is prime. 🙂👍

And now that we have a function able to know if a number is a prime number, all we have to do is to check all the numbers smaller that the user input, like so:

```C++
unsigned int largestPrimeSmallerThan (const unsigned int upperLimit)
{
    for(auto number = upperLimit; number > 1; --number)
    {
        if(isPrime(number))
        {
            return number;
        }
    }
    assert(false);
    return 1;
}
```

Since we want the largest prime which is smaller that the user input, we go from the user input and iterate down to 1 and stop if we identify a prime number.

## The UI interface on Android Studio

Now that we have some C++ function able to compute the solution of our problem, we have to create an interface enabling the user to give us the input we need.
The interface will be very similar to the one of our [first problem](https://10xlearner.com/2020/03/23/the-modern-c-challenge-on-mobile-the-first-problem/), so I won't go too much in the details.

Instead, I want to focus on a way to factorise the work we do when creating a new activity for a new problem.
Indeed, we can reduce the amount of work we do when creating a new activity for a new problem since the interfaces will have some similar elements.

To do so, we have created a layout file named `problem.xml` in which we placed all the elements that are going to appear in the interface of all problems such as the title, the text of the problem, and the button `NEXT` and `PREVIOUS`. Then, in the layout of each of the problem, we include the common `problem` layout like so:

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center_horizontal">

    <include layout="@layout/problem"/>
</LinearLayout>
```

Now that we have some elements included in all the problem, we must specify what they contains in the Activity Java code, instead of in the xml file of their layout.
For example, here is how the text of the problem 4 is specified:

```java
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_problem_4);

        // some code

        TextView problemText = findViewById(R.id.problemText);
        problemText.setText(getString(R.string.problem_4_text));

        // some other code
    }
```

Finally, we must for each problem activity to implements the callbacks of the buttons.
To do so, we can use an abstract class:

```java
public abstract class ProblemInterface extends AppCompatActivity {
    public abstract void goToNextProblem(View v);
    public abstract void goToPreviousProblem(View v);
}
```

And only implements the callback accordingly to the problem.

```java
public class Problem_4 extends ProblemInterface {
    @Override
    public void goToNextProblem(View v) {
        // empty on purpose
    }

    public void goToPreviousProblem(View v)
    {
        Intent intent = new Intent(this, Problem_3.class);
        startActivity(intent);
    }
}
```

And now, the creation of a new activity for a new problem will be much simpler since the common elements in all problem will be already defined and placed in the user interface. 🙂

## Using C++ native code

Linking the user interface and the C++ code will be similar to the previous problem. 😉
In the Java file of the activity, we declare the native method:

```java
public class Problem_4 extends ProblemInterface {
    public native String LargestPrimeSmallerThan(int userInput);
}
```

And in the native method, we call our c++ function to solve the problem and return the solution.

```C++
extern "C"
JNIEXPORT jstring JNICALL
Java_com_example_themoderncppchallenge_Problem_14_LargestPrimeSmallerThan(JNIEnv *env, jobject thiz,
                                                                          jint user_input) {
    auto result = largestPrimeSmallerThan(user_input);
    return env->NewStringUTF(std::to_string(result).c_str());
}
```

## Conclusion

Voilà ! We now have an application which can solve the first four problems of [The Modern C++ Challenge](https://amzn.to/2QdYmvA).

You can note that the solutions, written in this post, don't include all the sources to make a running program, but only the interesting part of the sources to solve this problem.
If you want to see the programs from end to end, you can go on my [GitHub account](https://github.com/Xav83/TheModernCppChallenge_AndroidStudio/tree/v0.0.4_FourthProblem), explore the full solution, add comments or ask questions if you want to, on the platform you read this article, it will also help me improve the quality of my articles.

Thank you all for reading this article,
And until my next article, have a splendid day 😉

![](https://github.com/Xav83/Xav83.github.io/raw/master/res/TheModernCppChallenge/Problem%20004.png "Final Application")

## Interesting links

- [The Modern C++ Challenge](https://amzn.to/2QdYmvA)
- [Github release](https://github.com/Xav83/TheModernCppChallenge_AndroidStudio/tree/v0.0.4_FourthProblem)
- [Prime number definition](https://en.wikipedia.org/wiki/Prime_number)
- [Reusing layout](https://developer.android.com/training/improving-layouts/reusing-layouts)
