---
layout:     post
title:      Guitar Chord and Tempo Recognition
subtitle:   
date:       2018-08-31
author:     Arthur
header-style: text
catalog: true
youtubeId: KX5zGTiGk58

tags:
    - MATLAB
    - MIR
---

## Introduction

This is a project that I've done independently during an internship at [Hotone Audio](http://www.hotoneaudio.com/index.html). 

It takes an input audio of guitar accompany, and can analyse the tempo and chord of each beat.

Here is a simple video about how the algorithm works:

{% include youtubePlayer.html id=page.youtubeId %}

## Design

![](/img/in-post/post-chord-system.png)

There are three modules in the system: `totalRunDetect()`, `detectBPMv4()` and `template_matchingv6()`.

`totalRunDetect()` is the main function that reads the audio file, and pass the parameters to `detectBPM()`. According to the feed back arguments BPM and onset_position, the main function will cut the input audio into slices at each beat of the audio and pass the slice to function `template_matching()` to recognize the chord and root of the audio slice. Finally, the main function will print out the result.

The tempo recognition function is `detectBPM()`. First, it will calculate the onsets of the audio. Using the onset information, the auto-correlation will be calculated. The autocorrelation result is further converted into the BPM scale. According to the maximum value, as well as the relative position of the peaks in BPM scale, the optimized result will be determined. This function can also make classification of 3/4 tempo and shuffle tempo.

The chord recognition function, `template_matching()`, uses the method of `Pitch Class Profile` to identify the chord. However, besides standard PCP extraction, there are many preprocessing and post processing steps in the algotirhm. For details, please check the document below.


## Document

Download the document [here](https://drive.google.com/open?id=1h7Q0KDRquEA_pHPDEsjpWz-8o6xt6Zkm).