---
layout:     post
title:      Guitar Based MIDI Controller 
subtitle:   
date:       2017-06-01
author:     Arthur
header-style: text
# catalog: true
youtubeId: QMB0MCbzzjw
tags:
    - MIDI

---

## Introduction

This is a final project of course *EE202 Digital Circuit*. Through a modified acoustic guitar, we can generate standard MIDI signal by a connected Arduino board.

Please watch the demonstration video:

{% include youtubePlayer.html id=page.youtubeId %}


## Design

We used a modified acoustic guitar, connect fret bars and strings to the Arduino pins.

![](/img/in-post/post-guitar2midi-guitar.png/)


When a certain fret is pressed, it will create a electric loop between specific fret and string, which can be detected by the arduino program.

![](/img/in-post/post-guitar2midi-arduino.jpg)

The program can further generate a standard MIDI signal, according to the number of fret and number of string that created the loop.


## Document

Download our Slides [here](https://drive.google.com/open?id=1KZcEP3naNxpu1xqWq-UIWpN1p6ZKAOBC).