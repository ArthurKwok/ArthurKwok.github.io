---
layout:     post
title:      Audio Signal Processing for Python
subtitle:   "Assignments and notes for Coursera Course: Audio Signal Processing for Music Application"
date:       2018-08-17
author:     Arthur
header-style: text
catalog: true
tags:
    - Python
---

## Prelude
Frequently used imports:

``` python
import numpy as np
import sys
import os
import matplotlib.pyplot as plt
from utilFunctions import wavread, wavplay, wavwrite

```

---

## A1
### Read audio file
when using `wavread()`, fs comes before signal, which is opposite of matlab function `audioread()`.

```python
def readAudio(inputFile):

    (fs,x) = wavread(inputFile)
   
    piece = x[50000:50010]

    return piece
```

### find maximum and minimum

```python
def minMaxAudio(inputFile):

    (fs,x) = wavread(inputFile)

    x_min = min(x)

    x_max = max(x)

    return x_min, x_max

```

### array indexing
when using indexing, the order is : 
`start:end:step`


```python
def hopSamples(x,M):

    x_down = x[0:x.size:M]
    
    return x_down
```

### downsampling
```python
def downsampleAudio(inputFile, M):
    """
    Inputs:
        inputFile: file name of the wav file (including path)
        	M: downsampling factor (positive integer)
    """

    (fs,x) = wavread(inputFile)

    x_down = x[0:x.size:M]

    wavwrite(x_down,fs/M,inputFile[0:len(inputFile)-4]+'_downsapled.wav')

```

---

## A2
### generate a sinusoid
```python
def genSine(A, f, phi, fs, t):
    """
    Inputs:
        A (float) =  amplitude of the sinusoid
        f (float) = frequency of the sinusoid in Hz
        phi (float) = initial phase of the sinusoid in radians
        fs (float) = sampling frequency of the sinusoid in Hz
        t (float) =  duration of the sinusoid (is second)
    Output:
        The function should return a numpy array
        x (numpy array) = The generated sinusoid (use np.cos())
    """

    n = np.arange(0,t,1/fs)

    s = A*np.cos(2*np.pi*f*n+phi)

    return s
```
testing:
```
s = A2Part1.genSine(A=1.0,f=40.0,phi0.0,fs=44100.0,t=1)
plt.plot(s)
plt.show()
```
![Screenshot from 2018-08-16 09-56-33.png-220.8kB][1]

### generate a complex sinusoid
use `1j` to calculate the imaginary number, `j` or `i` or `1i` is invalid.

`np.arange(0,N)` returns the array from 0 to N-1.


```python
def genComplexSine(k, N):

    cSine = np.exp(-1j*2*np.pi*np.arange(0,N)*k/N)

    return cSine
```
### implement DFT
the `*` returns point to point product of two `ndarray`.
if you want array or matrix product, use `np.prod.`

```python
def DFT(x):
    """
    Input:
        x (numpy array) = input sequence of length N
    Output:
        The function should return a numpy array of length N
        X (numpy array) = The N point DFT of the input sequence x
    """
    N = x.size
    X = np.array([])

    for k in range(0,N):
        cSine = np.exp(-1j*2*np.pi*np.arange(0,N)*k/N)
        prod = x*cSine
        X = np.append(X, sum(prod))
    
    return X
```

### implement IDFT
```python
def IDFT(X):
    """
    Input:
        X (numpy array) = frequency spectrum (length N)
    Output:
        The function should return a numpy array of length N 
        x (numpy array) = The N point IDFT of the frequency spectrum X
    """

    N = X.size
    x = np.array([])

    for n in range(0,N):
        cSine = np.exp(1j*2*np.pi*np.arange(0,N)*n/N)
        prod = X*cSine
        s = sum(prod)
        x = np.append(x, s/N)

    return 
```

### generate magnitude spectrum
```python
def genMagSpec(x):
    """
    Input:
        x (numpy array) = input sequence of length N
    Output:
        The function should return a numpy array
        magX (numpy array) = The magnitude spectrum of the input sequence x
                             (length N)
    """
    ## generate DFT
    N = x.size
    X = np.array([])

    for k in range(0,N):
        cSine = np.exp(1j*2*np.pi*np.arange(0,N)*k/N)
        prod = x*cSine
        X = np.append(X, sum(prod))

    ## compute magnitude
    magX = np.abs(X)

    return magX
```
