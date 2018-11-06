---
title: Android tips & tricks-draft
date: 2018-11-05 11:20:50 +0000
redirect_from: []
published: false

---
## Gradle Runner vs Android Build

In the following section, we'd like to give you a few use cases when to use our `Gradle Runner` and `Android Build` Steps and what the major differences are.

## About Gradle tasks

[Gradle tasks](https://docs.gradle.org/current/userguide/tutorial_using_tasks.html) are integral part of Gradle build script. They perform actions that are needed to build a project. A `gradle` task is a process you can run with `gradle`. You can run these tasks by running `gradle TASK-TO-RUN` in your Command Line / Terminal.

A standard Android Gradle project includes a lot of tasks by default such as:

* `androidDependencies` - Displays the Android dependencies of the project.
* `assemble` - Assembles all variants of all applications and secondary packages.
* `assembleAndroidTest` - Assembles all the Test applications.
* `clean` - Deletes the build directory.

### How to get the list of available Gradle tasks in your project

To get the basic task list, call `gradle tasks` in your Android app's directory. When running `gradle tasks`, you'll get a list of available Gradle tasks in the format:

    $ gradle task
    
    :tasks
    
    ------------------------------------------------------------
    All tasks runnable from root project
    ------------------------------------------------------------
    
    Android tasks
    -------------
    androidDependencies - Displays the Android dependencies of the project.
    signingReport - Displays the signing info for each variant.
    sourceSets - Prints out all the source sets defined in this project.
    
    Build tasks
    -----------
    assemble - Assembles all variants of all applications and secondary packages.
    assembleAndroidTest - Assembles all the Test applications.
    assembleDebug - Assembles all Debug builds.
    assembleRelease - Assembles all Release builds.
    ...

To see all the available tasks, call `gradle tasks --all`.

### Run Gradle task with our Steps

You can run any of the Gradle tasks on Bitrise either using `Do anything with our Script` Step, `Gradle Runner` or `Android Build` Step.

* You can run any of the tasks on Bitrise by using our `Script` step by calling `gradle task-name-to-run` (for example: `gradle assemble`). **sample**
* You can use our `Gradle Runner` or `Android Build` Steps and specify the task as the value of the step input.

![](/img/gradlew-gradle-task.png)

**Instead of running** `**gradle**` **directly, you should run the gradle commands through** `**gradlew**` **(Gradle Wrapper) which is part of our** `**Gradle Runner**` **Step.**

You can find more information about the Gradle Wrapper (gradlew) and how you can generate one (if you would not have done it already) in the [official guide](https://docs.gradle.org/current/userguide/gradle_wrapper.html).

## How to install an additional Android SDK package

You can update your current Android SDK package either using our Step or installing the respective package manually.

**Advantages of Install missing Android SDK components:**

**Advantages of Do anything with Script step:**

### Automatic installation of Android SDKs

We suggest you to use our `Install missing Android SDK components` Step to install dependencies and Android SDK components for your Android project. The Step runs the `gradlew dependencies` **command in your project**. Provide the required NDK version in the `NDK version` input field and let the step take care of installation!

![](/img/android-ndk.png)

### Manual installation of Android SDKs?

You can manually install the missing Android SDKs as well if you...

Before you start:

* Make sure you have the Android `sdkmanager` installed to your local computer. For more information on `sdkmanager`, check out their [guide](https://developer.android.com/studio/command-line/sdkmanager).

1. Add a `Do anything with Script step` to your workflow.
2. Use the Android `sdkmanager` tool to install the additional packages you need.

{% include message_box.html type="important" title="When to use the Script Step" content="
Please only use the `Do anything with Script step` solution if you really have to **WHENdoes that happen?**, as you'll have to manually update the `Script content` if the Android tools change. "%}

As an example, if you wanted to install the Android SDK v18 and the related `build-tools` v18.0.1:

1. Add the `Do anything with Script step` (can be the very first step in your workflow)
2. Type the following content:

       #!/bin/bash
       #fail if any commands fails
       set -e
       #debug log_
       set -x
       #write your script here
       sdkmanager "platforms;android-18"
       sdkmanager "build-tools;18.0.1"

You can check the full list of available packages (including obsolete packages) that you have already installed by running an `sdk` task:

1. Run `sdkmanager --list --include_obsolete --verbose` command.

   You can run this command on your own machine if you have `$ANDROID_HOME/tools/bin` in your `$PATH`.  If not, then you can run it with `/PATH/TO/ANDROID-SDK-HOME/tools/bin/sdkmanager ...`.

## Enable Gradle debug options

If your Gradle build fails, we recommend you to check your build's log in the `APPS & ARTIFACTS` tab. If you're lost, you can call `--stacktrace --debug` flags (for example, `gradle ... --stacktrace --debug`) to get more detailed logs.

In most cases `--stacktrace` should be enough, and the `Gradle Runner` step includes this flag by default. **why calling it if you can type it in in the debug section of Gradle Runner?**

![](/img/stacktrace.png)

## Run a bitrise Android build on your Mac/PC with Docker

You can run your build on your Mac/PC, inside the same `docker` container you use on [bitrise.io](https://www.bitrise.io), to fully test your build in an identical environment! You can find the detailed guide here: [How to run your build locally in Docker](/docker/run-your-build-locally-in-docker/)

## Memory (RAM) limit

You can specify the amount allowed RAM for the Java Virtual Machine by adding two **Environment Variables** to your Workflow, for example, as `App Env Var`s:

* `GRADLE_OPTS: '-Dorg.gradle.jvmargs="-Xmx2048m -XX:+HeapDumpOnOutOfMemoryError"'`
* `JAVA_OPTIONS: "-Xms512m -Xmx1024m"`

This method can be used to limit the allowed RAM the Gradle JVM process can use, which can be useful in case there's not enough RAM available in the system.

## Emulators

You can use our Android emulator [steps](http://www.bitrise.io/integrations) to create and boot Android emulators. Let's see how!

1. Add `Create Android emulator` Step to your workflow. **WHERE?**
2. Set the Android version in the `Target platform of the new AVD` Step input field. **ORHER INPUTS?**
3. Add the `Start Android emulator` Step to boot the new emulator. 
4. **Set the input ....**

### Emulator with Google APIs

**Instead of using a Script step to create an android emulator please use the** `Create Android emulator` **step__! There are simply too many edge cases to cover here, as well as the commands and working configurations change quite frequently.**

_The section below is kept here for referencing purposes, and might be outdated._

**Note about Android SDK versions:** at this time there are lots of known issues reported for Android Emulators with Android SDK version 22 & 23 when combined with Google Play services (see [1](http://stackoverflow.com/questions/32856919/androidstudio-emulator-wont-run-unless-you-update-google-play-services) and [2](https://code.google.com/p/android/issues/detail?id=176348)). The script above creates an emulator with SDK version 21, which should work properly with Google Play services.

_There are possible workarounds for newer versions (see_ [_1_](http://stackoverflow.com/questions/34329363/app-wont-run-unless-you-update-google-play-services-with-google-maps-api-andr) _and_ [_2_](http://stackoverflow.com/questions/33114112/app-wont-run-unless-you-update-google-play-services)), but that requires some customization in your project.

## Installing / Using Java version X

{% include message_box.html type="note" title="Java 8 is now pre-installed" content=" Java 8 is now the pre-installed Java version on the Bitrise.io Linux Stack. This section is kept here for future reference, in case you'd need another Java version. "%}

_If you'd need a Java / JDK version which is not preinstalled on the Android stacks, you can follow this guide to install it. This example will install Java/JDK 8, please adapt it to the version you need._

If your build requires JDK 8, you can install and activate it with a `Script` step:

    #!/bin/bash
    set -ex
    
    add-apt-repository -y ppa:openjdk-r/ppa
    apt-get update -qq
    apt-get install -y openjdk-8-jdk
    update-java-alternatives -s /usr/lib/jvm/java-1.8.0-openjdk-amd64
    echo "done"

That's all, just add the `Script` step to the Workflow with the content above, and start a new build. This `Script` step can be the very first step in the Workflow, as it does not depend on anything else.