# How to open a new Activity in a Android App

Hello ! I'm Xavier Jouvenot and in this small post, we are going to see how to open a new Activity in a Android App.

_Self promotion_: You can find other articles on my [website](www.10xlearner.com) 😉

## Problematic

> An activity is a single, focused thing that the user can do.

This quote from the [Activity class documentation](https://developer.android.com/reference/android/app/Activit://developer.android.com/reference/android/app/Activity) summarise well what is the purpose of one activity.
Since an Activity is _"a single and focused thing"_, and the majority of applications allows us to do several focused things, a question raised itself: how to go from one Activity to another ?
By answering this question, we are going to be able to craft application with several Activities where each one is a "single, focused thing that the user can do" 😉

## Solution

Create two activities

First of all, let's create two basic activities (you can only create one if you already have a default main activity):

![Creating a new Activity with Android Studio](https://github.com/Xav83/Xav83.github.io/raw/master/res/Android Studio/Create a Basic Activity.png "Creating a new Activity with Android Studio")

Then, once we have our two activities, we need to be able to have an event that is going to allow us to trigger the operation we want to do 😉.
To have the simplest example possible that I have in mind: we are going to use a button click.

We can easily to this step by adding a button to the layout of our first Activity. You can do that by using a drag-and-drop on the layout interface of the activity:

![Add a button to an Activity with Android Studio](https://github.com/Xav83/Xav83.github.io/raw/master/res/Android Studio/Add a button to an Activity.png "Add a button to an Activity with Android Studio")

, and by specifying the callback `onClick` either on the layout design interface, or in the code of the layout:

![Specifying a button onclick callback with Android Studio](https://github.com/Xav83/Xav83.github.io/raw/master/res/Android Studio/Button onclick callback.png "Specifying a button onclick callback with Android Studio")

or

```xml
<Button
  android:id="@+id/button_openOtherActivity"
  android:onClick="openOtherActivity"
  <!-- Rest of the button definition -->
/>
```

Finally, we can add the code to open the other Activity in the button callback:

```java
public void openOtherActivity(View v) {
  Intent intent = new Intent(this, OtherActivity.class);
  startActivity(intent);
}
```

And voilà, with you should be able to go from one Activity to the other without problem.
You can adapt the names of the Activity and of the button callback so that it fits your application and your classes purpose, but this is basically all you have to do before integrating this solution in your application 🙂

------------

Thank you all for reading this article,
And until my next article, have a splendid day 😉

## Interesting links

- [Introduction to Activities](https://developer.android.com/guide/components/activities/intro-activities)
- [Activity class documentation](https://developer.android.com/reference/android/app/Activity)

