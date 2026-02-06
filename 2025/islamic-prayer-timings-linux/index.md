---
date: 2025-07-06T20:20:00+03:00
draft: false
title: Islamic Prayer Timings for Linux
description: Islamic Prayer Timings & Hijri Date daemon for Linux with seamless Waybar integration. Displays a live countdown to the next prayer, shows all daily timings on hover, and sends a notification (with optional user script) at the Adhan.
categories:
  - Linux
  - Projects
  - Software
tags:
  - Linux
  - Waybar
  - Projects
comments: true
featuredImage: featured.webp
slug: islamic-prayer-timings-for-linux
authors:
  - abdo
---
For me, Knowing that 3 hours left until Fajr is more informative than knowing that it's 2 AM now. That was the motivation to create a Linux daemon and waybar custom module for Islamic prayer timings using Aladhan API.

## Requirements & Features
- Finding an [API](https://aladhan.com/) to get the correct timings for your location.

- Reading the config variables, **country**,  **city**, **hour24** and **onAdhan** from `~/.config/IslamicPrayerTimings`.

  This is a configuration file example:
  ```json
  {
    "country": "Egypt",
    "city": "Cairo",
    "hour24": false,
    "onAdhan": "~/.config/IslamicPrayerTimings/inAdhan.sh"
  }
  ```

- Making an update worker (thread) to get and store updated prayer timings and hijri date on program startup and at midnight.

- Caching the latest response in `~/.local/share/IslamicPrayerTimings/timings.json` in case starting the daemon without internet.

- Making a daemon that calculates the next prayer timing, time difference and starts a timer with the countdown.

- When the timer is finished, a notification will be sent and the user's shell script will run (optional) e.g. lock the screen, play a sound, .... This is example of such a script:
  ```shell
  #!/bin/bash
  # play adhan audio
  AUDIO_FILE="$HOME/.local/share/IslamicPrayerTimings/adhan-mansour-al-zahrani.mp3"
  mpv --no-video --loop=no --really-quiet "$AUDIO_FILE" &
  sleep 10
  # lock screen (for hyprland)
  hyprlock
  ```
  ![Islamic prayer timings notification](islamic-prayer-timings-notification.webp#center)
## Development
- Dependencies:
	- **g++** : C++ compiler
	- **CMake** : building tool for C++
	-  **nlohmann-json** : to parse json files
	- **curl** : to send http get request to retrieve the timings from the API
	- **libnotify** : for sending notification to a notification server
	- **Notification manager**: like swaync, mako, dunst or any other suitable notification server (notification daemon to receive and show notification)

- I'll retrieve the timings using an aladhan API :
`https://api.aladhan.com/v1/timingsByCity?city=Cairo&country=Egypt`
  
  Test the response using e.g. **postman**. I used `curl` to send a GET request to retrieve that response.

- The response is in JSON format, I used **nlohmann-json** for parsing it.

- AI tools like ChatGPT will be very useful to help in using libcurl, nlohmann-json and std::filesystem, generally saves the time of reading documentation or showing examples.

- To use a new thread to update the timings, a race condition may happens in case of reading the timings in the main thread before being fully written and updated by the update thread (even if this happens, it'll be corrected 1 second after).

- Before calculating the next prayer timing and starting the countdown, the timings must be fully fetched and updated first (the shared data) which happens in another thread, so a condition_variable will be used to ensure at the main thread that the update thread finished his work.

- For piping to waybar, I print (dump) json format in `stdout` contains the `text` and `tooltip` fields that Waybar custom module will be using.

## Waybar custom module
You can see the [wiki](https://github.com/Alexays/Waybar/wiki/Module:-Custom) for how to add a custom module to Waybar.
I want to show the next prayer timing name and a countdown timer. With tooltip on hover with the day prayer timings.
```json
  "custom/prayerTimings": {
    "exec": "$HOME/.config/waybar/modules/IslamicPrayerTimings/build/IPT -d",
    "format": "🕋 {text}",
    "return-type": "json",
    "tooltip": true
  }
```

You can find the code and installing instructions in my GitHub repo:


{{< github repo="abdalrahmanshaban0/Islamic-Prayer-Timings" showThumbnail=false >}}

