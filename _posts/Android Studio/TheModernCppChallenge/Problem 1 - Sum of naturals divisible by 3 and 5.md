# The Modern Cpp Challenge on Mobile - The First Problem

Hello ! I'm Xavier Jouvenot and here is the first part of a long series on [The Modern C++ Challenge](https://amzn.to/2QdYmvA).
In this article, I am going to explain how I solved the first problem in C++, and how I integrated the solution in an Android project.

The objective of this first problem is simple.
We must calculate the sum of all natural numbers divisible by either 3 or 5, up to some limit given by the user, and we have to print it.
The solution will be computed in C++ and the interface to get the user input and display the result will be handled with the Android Studio Framework.

## The C++ solution

As you may have seen in my previous blog post on [Android Studio installation with C++](https://10xlearner.com/2020/03/16/setting-up-android-studio-with-c-on-windows/).
We can use C++17 in Android Studio, so this is what I am going to use. 😉

As I start my journey in Android development, I have integrated my solution directly in the file `native-lib.cpp` created by default by Android Studio.
And here how the function looks like:

```C++
[[nodiscard]] constexpr auto sumOf3and5MultipleUpTo(const unsigned int limit)
{
    size_t sum = 0;
    for(auto number = limit; number >= 3; --number)
    {
        if(number % 3 == 0 || number % 5 == 0)
        {
            sum += number;
        }
    }
    return sum;
}
```

Let's take a closer look to this function, to make sure we all understand how it works and why it is that way.
First, let's see the prototype and what it gives us as information:

```c++
[[nodiscard]] constexpr auto sumOf3and5MultipleUpTo(const unsigned int limit)
```

This function is [constexpr](https://en.cppreference.com/w/cpp/language/constexpr), since the computation could be perform at compile time if we wanted to.
The return type is automatically deduced with the [auto](https://en.cppreference.com/w/cpp/language/auto) keyword.
It also as the attribute [[[nodiscard]]](https://en.cppreference.com/w/cpp/language/attributes/nodiscard) which means that the result of this function must be used by the program.
And finally, we have one input, which is a positive natural number, since it is of the type `unsigned int`.

One limit defined by this function is that the input should never be over `4294967295` which is the limit of the type `unsigned int`.
This limit could be over come by using a `size_t` or a big integer implementation, but, I don't know how to link those type to the Android Framework for now. 😝

After the prototype, this function declare and initialize the variable for the result of the computation:
```c++
size_t sum = 0;
```

This is a `size_t` variable to be sure that we can store the highest number possible, up to `18446744073709551615`.
If we wanted to be able to get even bigger numbers, we would have to use a big integer implementation.

Then, we have the computation of the result:
```c++
for(auto number = limit; number >= 3; --number)
{
    if(number % 3 == 0 || number % 5 == 0)
    {
        sum += number;
    }
}
```

The loop go down from the limit given by the user to `3`.
No need to check to `0`, because, neither `1` nor `2` or divisible by `3` or `5`, and even if `0` is divisible by them, adding `0` to a sum won't change the result.

With `number % 3 == 0`, we check if the number is divisible by `3`, and with `number % 5 == 0`, we check if the number is divisible by `5`.
And if one of this check is true, we add the number to the current computer `sum`.

And finally, we return the result:
```c++
return sum;
```

So here is my solution for this problem, let's see how the interface is done in Android Studio.

## The UI interface on Android Studio

Since this is the only problem this application solves for now, we only have one screen on our application, so I have specified the UI elements directly in the file `activity_main.xml` created by default by Android Studio.

Disclaimer : since I only start my journey into Android Studio and mobile development, the things I learn here and in the future post may be trivial for you.
There may even be some better ways than my personal solution to accomplish the same thing. If you have any of those ways in mind, please, tell me in the comments, so that I can improve myself. Thank you in advance 🙂

To create our UI, we are going to need two kinds of elements : [TextView](https://developer.android.com/reference/android/widget/TextView) and [EditText](https://developer.android.com/reference/android/widget/EditText).
The `EditText` is going to allow the user to give us the input, and the `TextView` will allow us to display the result to the user.
Here is what the xml of those elements look like:

```xml
<TextView
    android:id="@+id/result"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Result:"
    app:layout_constraintBottom_toBottomOf="parent"
    app:layout_constraintLeft_toLeftOf="parent"
    app:layout_constraintRight_toRightOf="parent"
    app:layout_constraintTop_toTopOf="parent" />

<EditText
    android:id="@+id/plain_text_input"
    android:layout_height="wrap_content"
    android:layout_width="match_parent"
    android:inputType="number"
    android:hint="Please enter a number."
    app:layout_constraintBottom_toBottomOf="parent"
    app:layout_constraintHorizontal_bias="0.498"
    app:layout_constraintLeft_toLeftOf="parent"
    app:layout_constraintRight_toRightOf="parent"
    app:layout_constraintTop_toTopOf="parent"
    app:layout_constraintVertical_bias="0.4"
    android:autofillHints="" />
```

Let's dig a little deeper in those elements.

First, the `EditText`.
It has a `android:id` attribute that specifies an id which will be useful when we will need to get the input given by the user in the `EditText`.
The attribute `android:inputType` is set to `number` which forces the user to enter numbers into the `EditText`.
And finally, the last attribute I want to talk to you about, the attribute `android:hint` which display a text to give a instruction or message to the user when `EditText` is empty

Now, let's look at the `TextEdit`.
Like the `EditText`, it has a `android:id` attribute that specifies an id which will be useful when need to display the result found by the cpp algorithm.
And we also have the attribute `android:text`, which is the text of the `TextView` displayed to the user. This is the attribute that we will modify with the result of the calculation done in C++. For now, it only displays "Result:".

We won't look to the other elements, since, there are not relevant for the purpose of this blog post, as they mainly specify the dimension and position of the elements.

## Using C++ native code

To link and use the C++ function we have created previously, we need to do several things.
First, we need to declare in the C++ file what we want to be accessible to the rest of the program.
It looks like the following code:

```c++
extern "C" JNIEXPORT jstring JNICALL
Java_com_example_themoderncppchallenge_MainActivity_Sum3And5Multiples(
        JNIEnv* env,
        jobject /* this */, const jint i) {
    auto sum = sumOf3and5MultipleUpTo(i);
    return env->NewStringUTF(std::to_string(sum).c_str());
}
```

This sample of code defines a function `Sum3And5Multiples` to be used in Java.
This function takes a `jint` in parameter, which will be the input of the user, give the parameter to out C++ function, and get the computed result before returning it as a string.
Here how in the Java code the prototype of this function looks like:

```java
public native String Sum3And5Multiples(int i);
```

Now, let's see how this function is used to display the result in the `Text View`, after getting the input of the user with the `Edit Text`.
Since our application only solve one problem for now, this code is written directly in the method `MainActivity::onCreate` inside the `MainActivity.java` file.
Here is how it looks like:

```java
EditText et = findViewById(R.id.plain_text_input);
et.addTextChangedListener(new TextWatcher() {
    @Override
    public void beforeTextChanged(CharSequence s, int start, int count, int after) {

    }

    @Override
    public void onTextChanged(CharSequence s, int start, int before, int count) {
        int limit = 0;
        if(count != 0)
        {
            limit =  Integer.parseInt(s.toString());
        }
        TextView tv = findViewById(R.id.result);
        tv.setText("Result: " + Sum3And5Multiples(limit));
    }

    @Override
    public void afterTextChanged(Editable s) {

    }
});
```

In this code, we get the `EditText`, by using its Id, and we attach a listener on its text.
```java
EditText et = findViewById(R.id.plain_text_input);
et.addTextChangedListener(new TextWatcher() {
```

Then, we specify what we want to do when the text inside the `EditText` change, when the user modify the input.
We start by checking if there is any input to get, if not, we compute the sum for `0` as an input.
```java
int limit = 0;
if(count != 0)
{
    limit =  Integer.parseInt(s.toString());
}
```

Once we have the input of the user, we get the `TextView`.
And finally, we call the C++ method and display its result.
```java
TextView tv = findViewById(R.id.result);
tv.setText("Result: " + Sum3And5Multiples(limit));
```

And voilà, we have link everything into a program that solves the first problem of [The Modern C++ Challenge](https://amzn.to/2QdYmvA).
Personally, I have added some `TextView` to give more information to the user. Here is how it looks:

![](https://github.com/Xav83/Xav83.github.io/raw/master/res/TheModernCppChallenge/Problem%20001.png "Final Application")

## Conclusion

This first problem and first real program in Android Studio was a great learning experience for me.
I really learn about a lot about Android Studio, and how the different parts of the program with native C++ works.

I know that I have a lot to learn, and I am excited to learn it as much as I enjoyed learning what I exposed in this article 🙂
Please, tell me if you have any suggestion or comment about some elements or about the solution I proposed.

Thank you all for reading this article,
And until my next article, have a splendid day 😉

## Interesting links

- [constexpr documentation](https://en.cppreference.com/w/cpp/language/constexpr)
- [TextView documentation](https://developer.android.com/reference/android/widget/TextView)
- [EditText documentation](https://developer.android.com/reference/android/widget/EditText)
- [GitHub full solution](https://github.com/Xav83/TheModernCppChallenge_AndroidStudio/releases/tag/v0.0.1_FirstProblem)
