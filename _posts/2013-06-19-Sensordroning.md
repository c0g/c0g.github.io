---
title: Sensordroning
date: 2013-06-19
tags: phd android
layout: post
---
###Shortform:

* Working on android app to log time-and-location stamped SensorDrone readings.
    - Currently bare-bones but working.
    - Longer term want to make this a generic citizen science sensor hub, where people can download sensing tasks off the App store and use their phones for good.
* GPSS in Sheffield took up all of my time last week, and was a great use of said time.
* Going to Scotland Sunday - Thursday next week.

###Longform:

After last week's pretty intense GP Summer School (it was great - I am trying to work out how to write about it while explaining how good it was without coming across as a moron), I decided to change tack a bit and work on my system to generate data using the Nexus 4 and SensorDrone.

As I grow more experienced programming Android, it starts to feel less like programming and more like being a telephone operator from some long ago time, routing messages between monolithic chunks of Google's code. If you want to do something, chances are Google has done it a few years ago and ten times better. Or, quite possibly, that thing is a stupid thing and you are bad for wanting to do it.

It does feel strange, coming from a background of mostly writing numerical code in scripting languages like Python or MATLAB. The Android platform does things in a specific way, and doing them any other way is either impossible, terrible or tedious.

I don't feel that nearly so much in numerical code, because I've already learned the platform. If I kept having to look up the meaning of $+$ I can imagine I'd get frustrated very quickly.

Back to the App. What's happening now is that I have an app that will run a SensorDrone, and keep querying it even when the phone is asleep, or the App is force closed. Necessary extensions are logging the data and sending it to a webservice (Google Cloud Messaging is favourite I think).

Monday was a write off as I got back up to speed with Java and Android, but with luck I should have something that can at least gather the data I need by Friday.

Unnecessary but cool extensions:

* Use Google's activity sensors and geofences to turn sensing on and off when the user leaves a certain region, or is doing a certain action like driving or walking. 
* Extend it to other sensing tasks. Using the phone's microphone for soundscape analysis is an excellent example of this.
* Make it pretty. Possibly the most important.

Also, I'm going to Islay next week to drink whisky and look at the ocean. Haggis may be involved. Android and Mathematics won't be.

