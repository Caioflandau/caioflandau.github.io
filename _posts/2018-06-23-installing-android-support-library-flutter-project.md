---
layout: post
title:  "Flutter/Android: Installing the Support Library on a Flutter project"
date:   2018-06-23 18:00:00 +0700
categories: Flutter,Android
permalink: "/installing-android-support-library-flutter-project"
---
In this small post, we'll look at how to install the Android Support Library on a Flutter project.
<!--more-->

# Why?
If you've worked with Android projects in the past, you may be used to having the Support Library for various backwards-compatible goodies. It comes for free when creating a regular Android project with Android Studio.

Flutter developers, on the other hand, chose not to include this. It's simply not needed for Flutter to work and increases the app size. So that makes a lot of sense.

But if you need to work on Android-specific code, chances are this library will be a requirement. So let's see how to get it set up!

# Prerequisites
This guide will assume:
- Your Flutter environment is set up (i.e. running `flutter doctor` doesn't report issues)
- You have a working Flutter project you created earlier (I recommend using Android Studio to create the project. Check [here](https://flutter.io/get-started/test-drive/#androidstudio) for instructions)

# Installing the Android Support Library on your Flutter project
For this part, we'll be using Android Studio as our tool of choice. While this can be done with Visual Studio Code, Android Studio makes it a lot easier on us.

## Open the correct project
The main "trick" here is opening the correct project in Android Studio:
1. On Android Studio, click `File > Open...` and navigate to your Flutter project's root. **Don't open it just yet!**
1. In your project's root folder, you will see a folder called `android`. That's the one we're looking for - open it
1. You will notice this is just a regular Android project. If you never opened it before, you may see an error message:
`Flutter SDK not found. Define location with flutter.sdk in the local.properties file.`
To fix it, as the message says, simply open your `local.properties` file in that same project and add:
```
# This is the full path to your flutter SDK - the one you downloaded from flutter.io:
flutter.sdk=<FLUTTER_SDK_PATH>
```
1. Android Studio should now be able to build it correctly

## Install the Android Support Library
Now that you have the right project opened and set up, installing the Support Library is simple. If you are used to Gradle, you can skip the step-by-step option and go straight to option 2. It's faster.

### Option 1: Step-by-step, using Android Studio to help
1. On the Project navigation on the left, right click the main project module (it's called `app` by default) and select `Open Module Settings`
1. On the left hand side of the window that just opened, make sure your module is selected under `Modules`. Again this is the `app` module by default
1. Open the "Dependencies" tab, click the `+` at the bottom left and select `Library Dependency`
1. Type `com.android.support:appcompat` in the search and press the search button on the right. You should get only one result. Double click it and press "Ok" to complete the change

Android Studio will trigger a Gradle sync for you.

### Option 2: Quick way (add the dependency directly to build.gradle)
What all the steps above do is simply find the latest version of the library and add the dependency to the `build.gradle` file. If you are used to Gradle, you can achieve the same by editing the `build.gradle` file yourself. At the time of writing, the dependency looked like this:
```
implementation 'com.android.support:appcompat-v7:27.1.1' <-- Make sure to use the right version. This was up-to-date as of writing this article
```

### That's it!

If you used option 2, make sure to trigger a Gradle sync now. You should then have access to the same support tools as you expect from a regular Android project.

And as the official docs say: Happy Fluttering! :)

## Bonus: Writing Android-specific code in your Flutter app project
If you open the `android` folder from the Android Studio window where the root Flutter project is open, it won't work correctly. Android Studio won't find the required SDKs and libs, and you won't get any help from the IDE.

But now that you [opened the correct project](#open-the-correct-project), you should be able to write code using all the right Android environment. Simply keep both windows open, and whenever you want to work on Android code, use the Android window. If you want to work on Flutter/dart code, use the Flutter window.

You can find more information on the [Flutter Plugin for IntelliJ docs for Android](https://github.com/flutter/flutter-intellij/blob/master/docs/android.md).