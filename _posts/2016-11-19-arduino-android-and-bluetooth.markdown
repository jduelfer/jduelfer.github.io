---
layout: 	post
title: 		"Arduino: Android Control via Bluetooth"
date: 		2016-11-19 18:00:00 +0200
categories:	Arduino, Android
---

This post will be about controlling the motors of a Arduino Robot Car with an Android App via a bluetooth connection. I saw the 
[HC-06](https://www.amazon.com/s/ref=nb_sb_noss_2?url=search-alias%3Daps&field-keywords=hc-06+bluetooth+module&rh=i%3Aaps%2Ck%3Ahc-06+bluetooth+module)
Bluetooth Module and thought that it would be a cool addition to my robot car project. The goal is to be able to control the car's 
functionality by an Android App. This post will specifically go over connecting the bluetooth module and provide a bit of Java 
code to be imported into an Android Studio project. Here is a quick video demonstrating what you will have by the end of the tutorial.

<iframe class="img-responsive video-responsive" src="https://www.youtube.com/embed/Bl-iQ6ohyzI" frameborder="0" allowfullscreen></iframe>

<br>

### Material
_______________________

The things that you will need for this tutorial are:

1. [HC-06](https://www.amazon.com/s/ref=nb_sb_noss_2?url=search-alias%3Daps&field-keywords=hc-06+bluetooth+module&rh=i%3Aaps%2Ck%3Ahc-06+bluetooth+module)
2. [Android Studio](https://developer.android.com/studio/index.html): you don't need to be an Android Developer but a little experience would be helpful.
3. 2 9V batteries: We want a little more power
4. [Battery Clip Connector](https://www.amazon.com/s/ref=nb_sb_noss_2?url=search-alias%3Daps&field-keywords=9v+battery+clip+connector): we are going to have two 9V batteries connected

<br>

### HC-06 Bluetooth Module
_______________________

This part is pretty easy to setup, as you are just connecting the Bluetooth module to the Arduino.

<br>

### Android Code
______________________

If you want to download the Android code for the controller, you can clone my [Github repo](https://github.com/jduelfer/BluetoothRobotCar) and build the App in Android Studio.