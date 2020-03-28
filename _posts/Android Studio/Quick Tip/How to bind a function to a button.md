# Quick Tip - How to bind a function to a button ?

Hello ! I'm Xavier Jouvenot and in this small post, I am going to explain how to bind a function to a button.

_Self promotion_: You can find other articles on Android development on my [website](https://10xlearner.com/category/android-studio/) 😉

## Handling your buttons click in the activity

There are two ways to handle your buttons click in your activity java code.

### Inheritance

The first method I want to talk to you about, can allow you to handle the clicks to all of the buttons of your activity in one function.
To do so, you should make your activity implements `View.OnClickListener` and override the method `onClick`:

```java
public class MainActivity extends AppCompatActivity implements View.OnClickListener{
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        // ...
        Button bt = findViewById(R.id.button);
        bt.setOnClickListener(this);
        // ...
    }

    @Override
    public void onClick(View v) {
        if (v.getId() == R.id.button) {
            // Do something
        }
    }
}
```

In this code, you specify that your button listener is your activity during the creating of the activity.
Then, when the user will click on the button, the method `onClick` will be called.
In that function, you will look at the id of the element to know which button has been clicked on, and react accordingly.

Personally, this is my least favorite method.
I found that, the more buttons you have in your interface, the less readable becomes the function `onClick`. 😢

### Binding a function on the Activity creation

This second method allows you to specifies your button callbacks during the activity creation.
It looks like that:

```java
public class MainActivity extends AppCompatActivity implements View.OnClickListener{
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        // ...
        Button bt = findViewById(R.id.button);
        bt.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                // Do something
            }
        });
        // ...
    }
}
```

The advantages of this method, compare to the first one, is that you don't have to use you button id to check what to do.
You know directly which button is concerned by your callback.

But the more buttons you have, the less readable your `onCreate` method will be. 😢

## Using the layout of the button

The last method I want to talk about consists of using the XML definition of the button to specify the callback of the button.
Indeed, you can specity which method of your activity you want to be called by adding an attribute `android:onClick` in the button XML definition.
This can look like this:

```xml
<Button
    android:id="@+id/button"
    android:onClick="onMyButtonClick"
    android:text="@string/button_text" />
```

```java
public class MainActivity extends AppCompatActivity implements View.OnClickListener{
    // ...
    public void onMyButtonClick(View v)
    {
        // Do something
    }
    // ...
}
```

Personally, I found this method to be the best one.
It allows you to separate the place where the callback is implemented from the place where it is linked to the button.
You don't have to check which button is clicked on since you know it from your XML, and you can create one callback for each of your button without making another method unreadable.

I definitely recommend you to use this one :wink:

--------------

Thank you all for reading this article,
And until my next article, have an splendid day 😉

## Interesting links

- [android:onClick documentation](https://developer.android.com/guide/topics/ui/controls/button#HandlingEvents)
- [onClickListener documentation](https://developer.android.com/guide/topics/ui/controls/button#ClickListener)
- [BeeApps french example](https://developpezvosappsandroid.com/button-onclick-exemple/)
- [10xlearner website](www.10xlearner.com)
