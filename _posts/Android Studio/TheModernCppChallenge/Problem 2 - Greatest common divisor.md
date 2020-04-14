# The Modern Cpp Challenge on Mobile - Greatest commont divisor

Hello ! I'm Xavier Jouvenot and here is the second part of a long series on [The Modern C++ Challenge](https://amzn.to/2QdYmvA).
In this article, I am going to explain how I solved the second problem in C++, and how I integrated the solution in an Android project.

The objective of this second problem is simple.
We must calculate the greatest common divisor of two positive integers given by the user, and we print it to the user.
The solution will be computed in C++ and the interface to get the user input and display the result will be handled with the Android Studio Framework.

I encourage you to read the [previous part of this series](https://10xlearner.com/2020/03/23/the-modern-c-challenge-on-mobile-the-first-problem/), since we are going to continue our program created in it.

## The C++ solution

Implementing the algorithm to find the greatest common divisor of two numbers is not simple... it literally given by the standard ! 😉
Indeed, in C++17, the function [std::gcd](https://en.cppreference.com/w/cpp/numeric/gcd) does exactly that !

So all we have to do, is to include the `<numeric>` header from the std and use the [std::gcd](https://en.cppreference.com/w/cpp/numeric/gcd) function.

If you are using an older version of C++ standard, then you will have to implement it.
I can recommend you this [Stack Exchange post](https://codereview.stackexchange.com/questions/66711/greatest-common-divisor) about it, if you want. 🙂

## The UI interface on Android Studio

Unlike for the C++ implementation of the solution, the User Interface is going to require some work.
Indeed, we want to be able to be able, in our application to have the solution for our first problem and for the second problem too.

In order to achieve this goal, we are going to need to do two things:
- Add a new Activity where the second problem will be displayed and the solution computed
- Add buttons to be able to go from the first problem to the second.

### A new Activity for a new problem

To create a new Activity, all you need to do is right click on your app folder architecture and go to `Empty Activity` like below:

![](https://github.com/Xav83/Xav83.github.io/raw/master/res/TheModernCppChallenge/New%20Activity.png "New Empty Activity")

Then, specify the name of your new Activity (I named mine `Problem_2`), and click on `Finish` to have your new Activity created.

Ok, but concretely, what does that mean ?
First of all, if you open the file `AndroidManifest.xml`, you can see that your activity has been declared here:
```xml
<activity android:name=".Problem_2"></activity>
```

Then, if you look into your Java files, a new file must have been created with a class named after your new activity.
This is the class in which we will handle the user input to give them to the C++ algorithm, before displaying the result to the user.

And finally, in the sub folder `layout` of your resources, you can see that a new xml has been added.
This xml will contain all the elements we want to display on the screen when the user will look at the second problem.

As in the [first problem](]https://10xlearner.com/2020/03/23/the-modern-c-challenge-on-mobile-the-first-problem/)), all we need are `EditText` and `TextView` to get the user input and display the solution to the user, so I encourage to read [it](https://10xlearner.com/2020/03/23/the-modern-c-challenge-on-mobile-the-first-problem/)) if you haven't.

### Linking the two activities

Now that we have created our new activity for our problem, we need to be able to access it through our application.
The simplest way to do that is to create a button on the first problem interface to go to the second problem interface.

To do so, we start by adding the callback, the function that will be called by the button, in the first Activity.
This function looks like that:

```java
public void goToNextProblem(View v)
{
    Intent intent = new Intent(this, Problem_2.class);
    startActivity(intent);
}
```

In this function, we create an `Intent` and feed it the class of our second activity.
Then, we start this activity. Pretty straightforward, isn't it ?! 🙂

Now that we have our callback, let's create a button in our activity, and link it to our callback.
This can be achieve by adding the following element to the layout of the first activity:
```xml
<Button
    android:id="@+id/button"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:onClick="goToNextProblem"
    android:text="@string/button_next"
    app:layout_constraintBottom_toBottomOf="parent"
    app:layout_constraintEnd_toEndOf="parent"
    app:layout_constraintHorizontal_bias="0.95"
    app:layout_constraintStart_toStartOf="parent"
    app:layout_constraintTop_toTopOf="parent"
    app:layout_constraintVertical_bias="0.954" />
```

What is important here is that we have the field `android:onClick` set with our callback's name.
That way, when a user will click on the button, it will trigger our function.

You can do the same thing in the second activity to be able to go back to the first activity.
And voilà, you have a proper user interface to the two first problem.
All we have to do is to link the user interface to the C++ algorithm.

## Using C++ native code

In this part, we are going to look at the Java class of the second activity.

```java
public native String Gcd(int i, int j);

private int extractNumberFromEditText(EditText et)
{
    assert et.getText().length() != 0 : "The EditText must not be empty";
    return Integer.parseInt(et.getText().toString());
}

private void computeAndDisplayResult()
{
    EditText et1 = findViewById(R.id.first_input_number);
    EditText et2 = findViewById(R.id.second_input_number);

    int firstNumber = extractNumberFromEditText(et1);
    int secondNumber = extractNumberFromEditText(et2);

    TextView tv = findViewById(R.id.result);
    tv.setText(getString(R.string.result_placeholder, Gcd(firstNumber, secondNumber)));
}
```

If you have read my [blog post about the resolution of the first problem](https://10xlearner.com/2020/03/23/the-modern-c-challenge-on-mobile-the-first-problem/), this part doesn't bring new.
First, We declare the function `Gcd` linked in the C++ library.
Then we have a method which get the `EditText` with the user input, we extract the input from them, we get the `TextView` where we are going to display the result, we compute the result and we display it.

But unlike in the first problem, this code is in private function. Not in `EditText` callback.
This is because of one important thing. We can't compute the GCD algorithm if there is only one input user.
We must make sure to have both of the user input filled.

So here is how the callback looks like:
```java
EditText et = findViewById(R.id.first_input_number);
et.addTextChangedListener(new TextWatcher() {
    @Override
    public void beforeTextChanged(CharSequence s, int start, int count, int after) {

    }

    @Override
    public void onTextChanged(CharSequence s, int start, int before, int count) {
        EditText et = findViewById(R.id.second_input_number);
        if(et.getText().length() == 0 || count == 0)
        {
            return;
        }
        computeAndDisplayResult();
    }

    @Override
    public void afterTextChanged(Editable s) {

    }
});
```

So for the first `EditText`, when the text changes, we check is the new text is not empty (`count == 0`) and is the second `EditText` is not empty (`et.getText().length() == 0`).
If none is empty, we call our function to compute and display the result. If one is empty, we do nothing by returning directly.
For the second `EditText` the code is similar, only the `EditText` ids change. 😉

## Conclusion

So, we now have an application which can solve the first two problems of [The Modern C++ Challenge](https://amzn.to/2QdYmvA) and learned new thing on Android Studio which, for this problem, was the real purpose for me, since the C++ algorithm is already integrated in the C++ standard.

You can note that the solutions, written in this post, don't include all the sources to make a running program, but only the interesting part of the sources to solve this problem.
If you want to see the programs from end to end, you can go on my [GitHub account](https://github.com/Xav83/TheModernCppChallenge_AndroidStudio/tree/v0.0.2_SecondProblem), explore the full solution, add comments or ask questions if you want to, on the platform you read this article, it will also help me improve the quality of my articles.

Thank you all for reading this article,
And until my next article, have a splendid day 😉

![](https://github.com/Xav83/Xav83.github.io/raw/master/res/TheModernCppChallenge/Problem%20002.png "Final Application")

## Interesting links

- [The Modern C++ Challenge](https://amzn.to/2QdYmvA)
- [std::gcd documentation](https://en.cppreference.com/w/cpp/numeric/gcd)
- [Github release](https://github.com/Xav83/TheModernCppChallenge_AndroidStudio/tree/v0.0.2_SecondProblem)
