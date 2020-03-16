# Setting up Android Studio with C++ on Windows

Hi dear reader, I'm Xavier Jouvenot and in this article, we are going to talk about how to set up Android Studio to have it run the default c++ project.
This is the beginning of a series of blog post in which I will experiment with Android Studio to create some application I have in mind, and to complete the challenges from *[The Modern Cpp Challenge](https://amzn.to/39MWIJm)*, by Marius Bancila.

## Installation

First of all, we must install Android Studio on our machine.
I will only talk about the Windows installation, as I personally only have a Windows machine.

### Via the website

To download Android Studio, all you have to do is go to the [Android Studio website](https://developer.android.com/studio) and click on "Download Android Studio".
Once downloaded, run the installer and select the the setup and the theme that you want.
Personally, I selected a standard setup and the dark theme.

### Via a package manager

Another way to install Android Studio on Windows, is to use a package manager.
I personally tend to use [Chocolatey](https://chocolatey.org/why-chocolatey), and I really encourage you to give it a try, if you never have. :)

To install Android Studio with [Chocolatey](https://chocolatey.org/why-chocolatey), all you need to do is to run the command `choco install androidstudio` in the Powershell command, and accept the installation. You can find the details of Android Studio package for Chocolatey [here](https://chocolatey.org/packages/AndroidStudio), if you want.

Once installed, you can open Android Studio and select the the setup and the theme that you want.
Personally, I selected a standard setup and the dark theme.

## Project configuration

Now that you have an Android Studio ready to be used, let's configure and run successfully a C++ project in Android Studio.
First of all let's create a new project by going into the menu `File > New > New Project`.
In the windows that appeared, select the C++ project.

![](https://github.com/Xav83/Xav83.github.io/raw/master/res/Android%20Studio%20Installation/01%20-%20Select%20a%20project%20template.png "Project Template selection")

Then configure your project location and name.

![](https://github.com/Xav83/Xav83.github.io/raw/master/res/Android%20Studio%20Installation/02%20-%20Configure%20your%20project.png "Project configuration")

Finally, you can select the your cpp standard. I personally went for the latest available, C++17.

![](https://github.com/Xav83/Xav83.github.io/raw/master/res/Android%20Studio%20Installation/03%20-%20Customize%20Cpp%20Support.png "Cpp standard selection")

Now, you should have a default cpp project available to you in Android Studio. 💪😃
All we need to do is to be able to run it.
To do so, we first have to sync our project with Gradle Files by clicking on this icon ![](https://github.com/Xav83/Xav83.github.io/raw/master/res/Android%20Studio%20Installation/04%20-%20Sync%20Project%20With%20Gradle%20Files%20Icon.png "Sync Project With Gradle Files Icon")

At that point, either the synchronization is a success, or it is not.
If it did fail, I suggest you to read this [article about how to set install NDK in Android Studio](https://10xlearner.com/2020/03/16/how-to-set-up-ndk-in-android-studio/), to make your synchronization successful, and to come back after to finish to setup your project.

So now that you have synchronize you project with Gradle, all we have left to do is to run the project on an emulator.
And for that, we need to specify the device we want to emulate.
To do so, we must click on ![](https://github.com/Xav83/Xav83.github.io/raw/master/res/Android%20Studio%20Installation/05%20-%20AVD%20Manager%20Icon.png "AVD Manager Icon") to open the AVD Manager (*AVD* stands for *Android Virtual Device*). You should see the following window.

![](https://github.com/Xav83/Xav83.github.io/raw/master/res/Android%20Studio%20Installation/06%20-%20AVD%20Manager.png "AVD Manager")

On this windows, click on the button `+ Create Virtual Device...` and select the device of your choice.
Personally, I took the Pixel 2, since it is my current phone.
After selecting your device, you can use the recommended parameters for the device and validate its creation.

And finally, you can hit run to launch the program, either by using the icon ![](https://github.com/Xav83/Xav83.github.io/raw/master/res/Android%20Studio%20Installation/07%20-%20Run%20Icon.png "Run Icon"), by going into the menu `Run > Run 'app'` or by using the shortcut `Maj + F10`.
Then you wait a little bit, and you have it, an Hello World into your phone emulator.

![](https://github.com/Xav83/Xav83.github.io/raw/master/res/Android%20Studio%20Installation/08%20-%20Hello%20World.png "Hello World")

## Conclusion

After all this, we are ready to finally start coding, but that will be for another post 😜

Personally, I had more problem that this post shared since I had to deal with the fact that my main hard drive is almost full, so I have to change most of the default installation paths in Android Studio.
I hope this post will help you a little in your installation and your first steps to mobile development. 🙂


Thank you all for reading this article,
And until my next article, have an splendid day 😉

## Interesting links

- [Setup NDK in Android Studio](https://10xlearner.com/2020/03/16/how-to-set-up-ndk-in-android-studio/)
- [How to modify your Android Sdk folder](https://chrisrisner.com/Changing-the-SDK-Path-with-Android-Studio)
- [How to modify your Android emulators folder](https://www.mysysadmintips.com/windows/clients/761-move-android-studio-avd-folder-to-a-new-location)
- [GitHub project](https://github.com/Xav83/TheModernCppChallenge_AndroidStudio/tree/helloWorld)
