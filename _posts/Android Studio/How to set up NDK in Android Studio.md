# How to set up NDK in Android Studio ?

Hi dear reader, I'm Xavier Jouvenot and in this article, we are going to talk about how to set up NDK in Android Studio.
This is directly related to the blog post about "[Setting up Android Studio with C++ on Windows](https://10xlearner.com/2020/03/16/setting-up-android-studio-with-c-on-windows/)", since this is a problem I faced when installing Android Studio on my machine

## A little bit of context

When syncing your project with Gradle Files, you may have encountered an error stating that you need some version of the NDK (for information, *NDK* stands for *"Native Development Kit"*). At least, I did, when I first installed Android Studio.

So this post will be available here for anyone facing the same problem or for my future self 😉

## The process

To set up the right NDK for you, you must first know which version of the NDK you want, or have to install.
For me, it was easy, because when I sync my project with Gradle Files, the error displayed the version of the NDK required.

Once you have this information, open the SDK Manager by clicking this icon ![](https://github.com/Xav83/Xav83.github.io/raw/master/res/Android%20Studio%20Installation/09%20-%20SDK%20Manager%20Icon.png "SDK Manager Icon"), or in the menu `Tools > SDK Manager`.

On the SDK Manager window, select the SDK Tools tab.

![](https://github.com/Xav83/Xav83.github.io/raw/master/res/Android%20Studio%20Installation/10%20-%20SDK%20Tools.png "SDK Tools")

(Side note : on this Windows, you can edit the android sdk location if, like me, you are too short on your main C: hard drive)

Now that you are on the SDK Tools tab, you should see a `NDK (Side by side)` checkbox.
Before selecting it, you must first select the checkbox `Show Packages Details`, to display all the version available of the packages.
And now, you can select the version of the `NDK (Side by side)` that you want.
Personally, the version required for me was the version `20.0.5594570`.

## Conclusion

Now, you and I know how to install whatever NDK package we may need in the future, and we know a little bit more about Android Studio.
I hope this post helped you a little in your Android Studio journey. 🙂

Thank you all for reading this article,
And until my next article, have an splendid day 😉

## Interesting links

- [Setting up Android Studio with C++ on Windows](https://10xlearner.com/2020/03/16/setting-up-android-studio-with-c-on-windows/)
- [How to modify your Android Sdk folder](https://chrisrisner.com/Changing-the-SDK-Path-with-Android-Studio)
- [How to modify your Android emulators folder](https://www.mysysadmintips.com/windows/clients/761-move-android-studio-avd-folder-to-a-new-location)
