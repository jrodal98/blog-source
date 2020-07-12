---
title: "Launching Scripts from a Smartwatch"
date: "2019-12-15"
description: "A guide on how to execute shell commands from a smartwatch"
tags: [
"Linux",
"watch",
"bash",
"reddit",
]
type: "post"
---

This article explains how to execute shell commands on a Linux computer from an android smartphone and a WearOS smartwatch.
<!--more-->


## TL;DR

* Linux computers can communicate with android smartphones using [KDEConnect](https://kdeconnect.kde.org/).
* If you purchase the tasker android app for $3.49, you can set up a notification task that can be pushed to your smartwatch.
* I was awarded the r/unixporn post of the month for a demo video that I posted [here](https://www.reddit.com/r/unixporn/comments/e65cb1/ticwatch_pro_launching_scripts_from_smartwatch/).

## Launching scripts from an Android device

Before you can launch scripts from a smartwatch, you need to be able to launch scripts from a smartphone. The instructions below are for an arch linux based distribution running i3WM, but there are plenty of guides on the internet - just search for "How to install KDEConnect on \<insert your distro here\>."

1) Install KDEConnect on your Linux machine with `sudo pacman -Syu kdeconnect`
2) Add `exec --no-startup-id /usr/lib/kdeconnectd` to your i3 config
3) Restart your i3 session
4) Download the kdeconnect app on your android device. Open it.
5) Open up the kdeconnect application on Linux and pair to your android device.
6) Add commands that you want to run. Change other settings as you please.

At this point, you should be able to run commands from your phone. Consider adding the KDEConnect command widget to your android home screen. If you don't know what I'm talking about, Google "android widgets" or something like that.

## Launching scripts from Wear OS smartwatch

* I could only figure out how to launch scripts from a smartwatch by using Tasker, which costs $3.49. It's a pretty cool app though with tons of functionality, so it's a fair deal. You might have luck with free alternative android applications, such as Automate or MacroDroid. Here's the Tasker strategy:

1) Download Tasker from Google Play
2) For each KDEConnect command that you want on your watch (a maximum of 3):
    1) Copy the command URL by pressing and holding the command
    2) Go to the tasks tab on Tasker
    3) Add the following action: Net -> Browse URL. Paste your URL in the prompt.
    4) Click the add button and name the task whatever you want it to be
3) Once you have your tasks ready, add another task that will serve as the notification dialogue
    1) Add the following action: Alert -> Notify
    2) Fill out title and optionally the text, add notification icon, etc. Make sure that the notification is not set to permanent. At the bottom of the screen, you'll see "Actions." Add your tasks from the previous step (the KDEConnect commands) here.
    3) At the bottom of the screen, you'll see "Actions." Add your tasks from the previous step (the KDEConnect commands) here.
    4) Repeat 1-3 for each of your KDEConnect command tasks.
4) Launch the notification task in Tasker by clicking on its name under the Tasks tab and hitting the play button. This will send it to your watch's notification screen.
