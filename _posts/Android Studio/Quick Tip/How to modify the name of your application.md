# Quick Tip - How to modify the name of your application ?

Hello ! I'm Xavier Jouvenot and in this small post, I am going to explain how to modify the name of your application.

_Self promotion_: You can find other articles on Android development on my [website](www.10xlearner.com) 😉

## Finding your application title

To be able to change the title of an android application, we have to know where the application title is handled by default.

If you open the `AndroidManifest.xml` file of your application, you will find a line looking like `android:label="@string/app_name"` in the element `application`.
This attribute is the one we are looking for, and, in that case, it sets the name of an application using a `string` stores in your resources named `app_name`.

If you typed your title directly in the `AndroidManifest.xml` file, like `android:label="My Wonderful Application Title"`, it would certainly work.
But a best practice would be to modify it in your resources file, so that, if you need to use your application name, you would only have to change it once.
Moreover, if you want to support several languages and adapt your application title depending on the country, it would be easier if it is stored in the resources file.

## Into the resources file !

By default, your application name can be found in the `strings.xml` file of your resource folder

![](https://github.com/Xav83/Xav83.github.io/raw/master/res/Android%20Studio/String%20Resouces%20file.png "String Resource File")

In this file, you will find a line looking like : `<string name="app_name">MyApplicationName</string>`

By changing `MyApplicationName` by whatever you want, then you will modify the title of your application.

Moreover, you can also use this resource to use your application in other places.
Indeed, you can either use `@string/app_name` to use your application name in the attribute of some field in your layouts, or in the Java code of your application, you can get the title by using the `R` object and the `getString` function, like so : `getString(R.string.app_name)`.

--------------

Thank you all for reading this article,
And until my next article, have a splendid day 😉

## Interesting links

- [Application manifest documentation](https://developer.android.com/guide/topics/manifest/application-element)
- [getString documentation](https://developer.android.com/reference/android/content/res/Resources#getString(int,%2520java.lang.Object...))
- [10xlearner website](www.10xlearner.com)
