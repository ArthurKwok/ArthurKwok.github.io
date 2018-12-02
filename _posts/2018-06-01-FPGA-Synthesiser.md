---
layout:     post
title:      FPGA Synthesiser & Sequencer
subtitle:   
date:       2018-06-01
author:     Arthur
header-style: text
# catalog: true
youtubeId: iXJiPQbhQuA
tags:

---

## Introduction

This is a final project of course *EE332 Design of Digital System*. The aim of the course is to teach VHDL, as well as the methodology of designing a digital system, based on the knowledge of digital circuits.

For the lab work, the course uses *Nexys 4 DDR* develop board, and *Vivado* as the software.

Here is a simple video about how our program works:

{% include youtubePlayer.html id=page.youtubeId %}

The board is connected to a speaker from the audio jack.

## Design

This is the features of the board, according to the official document:


![](/img/post-FPGA-plot.png)

Our program uses No.8, switches as the main control. There are 16 switches in total, each one represent a beat. Turn the switch up means this tatum is currently being edited. You can edit multiple beats at a time.

No.7 are the LEDs. It will show the beat that is now playing, if the current tatum has a sound, the LED will turn on.

No.11, the five buttons are used to edit the beats. every beat has 16 selectable pitch, selected by pressing the up button. After selection, press right button to save the pitch into editable beats(swtich up). Press center button to stop/play.

No.9, the 7 segment display, will show the current pitch, from 1 to F. The pitches are in a pentatonic scale, three octaves of C,D,E,G,A, and a C in the fourth octave.

No.17 is the audio jack. It uses PWM for DA convertion. The output waveform can be squarewave or sine wave, but we can only change the output in the code.


## Document
For technical details, please download our report.

Download our course project report [here.](https://drive.google.com/open?id=1XROKcsPyorzjFuNwu0GC86kFPOFhOiEx)
