---
layout: post
title: Green-screen webcam on Linux
---

Having spent a large amount of time in video calls, for work and play, and being relatively familiar with many video editing techniques the idea of replacing my webcam background was always alluring. Having poked around the topic a few times there were not too many straightforward techniques for Linux much less producing a high quality result. Having utilized [Open Broadcaster Software (OBS)](https://obsproject.com/) fairly extensively I focused on trying to stream the output to a [V4L2](https://en.wikipedia.org/wiki/Video4Linux) [loopback](https://github.com/umlaeute/v4l2loopback) device.

## Background

The idea was relatively simple, instead of outputting to a file or streaming service output to the loopback device. Then the loopback device could serve as my webcam in any video call application. Unfortunately, no matter what combination of settings were used it was not possible to write to the loopback device using the options available in OBS. Using an [`ffmpeg`](https://www.ffmpeg.org/) one can create a local _rtmp_ server that outputs to a V4L2 loopback device.

    ffmpeg -re -listen 1 -i rtmp://0.0.0.0:13377/ -f v4l2 /dev/video17

OBS can then be configured to stream to that address. That works, but has the downside of several seconds of delay between the camera input and the output on the loopback device. The delay is reasonable considering the stream is being encoded and decoded several times with a buffer at each step.

Being aware of [CatxFish/obs-virtual-cam](https://github.com/CatxFish/obs-virtual-cam) I decided to go about writing a V42L plugin for OBS, but thankfully I searched once again and found [CatxFish/obs-v4l2sink](https://github.com/CatxFish/obs-v4l2sink) had been created just a couple months before hand.

## Solution

Since my machines run [openSUSE Tumblweed](https://software.opensuse.org/distributions/tumbleweed) for which I already maintain the [obs-studio package](https://pmbs.links2linux.de/package/show/Multimedia/obs-studio) I created an [obs-v4l2sink package](https://pmbs.links2linux.de/package/show/Multimedia/obs-v4l2sink). Everything else needed was already provided in the main repository.

    zypper in obs-v4l2sink v4l2loopback-kmp-default

Given there are a number of steps that need to be performed to setup the camera and that I would be doing them repetitively I went ahead and created the following script which I then added to my tray.

![tray icon](/files/2020-04-14/tray-icon.png)

```bash
#!/bin/bash

if [ ! -e /dev/video17 ] ; then
  kdesu modprobe v4l2loopback video_nr=17 exclusive_caps=1 card_label="Green-screen Webcam"
fi

obs --profile "1080p @ 30 (vaapi)" --collection webcam-1080p --scene webcam-green-screen
```

Based on experimentation with different browsers `exclusive_caps=1` and `card_label="..."` are needed to have your webcam work everywhere. My script is explicit with the profile, collection, and scene since I use OBS for a variety of other things so I need the appropriate combination for use with webcam.

Within OBS the _obs-v4l2sink_ manifests plugin itself under _Tools -> v42lsink_ menu item.

![obs v42lsink menu](/files/2020-04-14/obs-v4l2sink-menu.png)

![obs v42lsink dialog](/files/2020-04-14/obs-v4l2sink-dialog.png)

_Auto Start_ is not useful for me since it applies globally so when using OBS for other tasks it still auto starts. Otherwise, that would eliminate having to click _Start_ everytime, but the option may fit into your workflow.

## OBS essentials

There are plenty of tutorials and documentation on setting up OBS, but for those unfamiliar I will include a few essentials relevant to using it for a green-screen webcam.

Configure the _Video_ based on your desired output resolution. Depending on your webcam, size of your green-screen, and positioning of the two you may want to use different values, but otherwise set them to 1080p or 720p depending on your webcam.

![obs video settings](/files/2020-04-14/obs-video-settings.png)

Add a _Video Capture Device (V4L2)_ to your scene.

![obs video capture device](/files/2020-04-14/obs-video-capture-device.png)

Add _Chroma Key_ under _Effect Filters_ and adjust to your liking. Then add a desired background image, video, etc to the scene behind the webcam source.

Depending on the aperture angle of your camera and the distance/size of green-screen you may need to add a _Crop/Pad_ filter as well to reduce the source feed size within your scene.

Once complete you should have a new camera device in your browser (may need to restart it) and be able to achieve something like the following.

![html5 webcam test](/files/2020-04-14/html5-webcam-test.png)

## Green-screen

Originally, I used a white wall, but due to my complexion, clothing, and lighting it did not work as well as desired. Using an un-ironed, $15 green-screen with a basic white LED light in my ceiling fixture I was able to achieve a nearly perfect result.

## Multiple backgrounds

I eventually added different backgrounds via multiple scenes and have my startup script randomly pick one by replacing the `obs` command with the following.

```bash
scenes=(
  abstract
  blue-particle-glow
  dark-cinima
  gold-lights
  lightbeams
)

scene_index=$(($RANDOM % ${#scenes[@]}))
scene="${scenes[$scene_index]}"

obs --profile "1080p @ 30 (vaapi)" --collection webcam-1080p --scene "$scene"
```

## Encoding oddities

With one of my first backgrounds, a static image, I encountered folks indicating that my background was _"flashing"_. The video on my end, even in the video call application, did not exhibit the behavior. After calling myself I was able to reproduce the results and concluded that the re-encoding done server side was causing the effect. I resolved it by using a _busier_ background instead of one with a large solid color for the majority.

## Conclusion

Having used this setup for roughly two years without issue it has been worth the effort to provide a snazy background instead of a boring wall or random room background. It has also amused many others with whom I have had calls.
