---
title: Public sample apps-draft
date: 2018-10-30 14:13:21 +0000
redirect_from: []
published: false

---
This guide aims to remedy some of the most frequent pain points you might experience when configuring your workflow. With the help of our Bitrise public sample apps we will demonstrate how a primary and a deploy workflow should be configured, and what a successfully run build looks like.

{% include message_box.html type="note" title="My message" content=" All the sample apps we provide in this guide are monitored by our developers on a weekly basis. All apps are scheduled to run between 4 and 5 on every Monday morning to see if the VM updates happening on Saturdays have disrupted the sample apps. If so, our developers fix the sample apps so that you have them as reliable reference."%} 

carthage-sample-app

Our `Carthage` Step is a iOS dependency manager. 

* Make sure you add your `Github Personal Access Token` input in the step as a secret, otherwise the build will throw the following error message: API rate limit exceeded for 208.52.166.154. (But here’s the good news: Authenticated requests get a higher rate limit. Check out the documentation for more details. 
* The step must come before any building step! 

xamarin-sample-app

* NuGet restore Step is the recommended dependency manager for your Xamarin project.

ionic-sample-app and cordova-sample-app

* `Generate cordova build configuration` step is a configuration step which generates the build.json file on which the building is based.

For more information on code signing Ionic or Cordova, read this guide.

For more information on how to get stared with Ionic or Cordova, read this guide.

fastlane-ios-sample-app and fastlane-android-sample-app

fastlane-snappy-sample-app

This workflow is configured to create screenshots of the unit test

android-sample-app

* in your primary workflow you should have all the testing steps such as `Android Lint` and `Android Unit Test` Steps, whereas your deploy workflow should contain the building step, like Android Build or Gradle Runner Steps.