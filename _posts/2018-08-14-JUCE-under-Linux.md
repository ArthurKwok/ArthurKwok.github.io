---
layout:     post
title:      JUCE configuration under ubuntu18.04/VSCODE
subtitle:   
date:       2018-08-17
author:     Arthur
header-style: text
catalog: true
tags:
    - Linux
    - JUCE
    - C++
---

## JUCE 配置
### 软件下载
https://juce.com/

### 依赖环境
```
sudo apt-get -y install g++ 
sudo apt-get -y install libfreetype6-dev 
sudo apt-get -y install libx11-dev 
sudo apt-get -y install libxinerama-dev 
sudo apt-get -y install libxrandr-dev 
sudo apt-get -y install libxcursor-dev 
sudo apt-get -y install mesa-common-dev 
sudo apt-get -y install libasound2-dev 
sudo apt-get -y install freeglut3-dev 
sudo apt-get -y install libxcomposite-dev
```

## C++环境配置

```
sudo apt-get install build-essential
sudo apt-get install g++-4.8
```

## VSCODE配置
在VSCODE palette中执行：
```
ext install cpptools
ext install code-gnu-global
```
在terminal中执行：
```
sudo apt-get install global
```

在项目文件夹下的 ./.vscode文件夹中新建：
`c_cpp_properties.json`:
```json
{
    "configurations": [
        {
            "name": "Linux",
            "includePath": [
                "${workspaceFolder}/**",
                "~/Documents/Workspace/JUCE/software/modules"
            ],
            "defines": [],
            "compilerPath": "/usr/bin/gcc",
            "cStandard": "c11",
            "cppStandard": "c++17",
            "intelliSenseMode": "clang-x64"
        }
    ],
    "version": 4
}
```
`tasks.json`:
```
{
{
    "version": "2.0.0",
    "tasks": [
        {
            "label": "build",
            "type": "shell",
            "command": "make",
            "args": [
                "-C","./Builds/LinuxMakefile/"
            ],
            "problemMatcher": [
                "$gcc"
            ]
        }
    ]
}
```
`launch.json`:
将`"program"`中的`MyGUI`替换为项目名。
```{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "(gdb) Launch",
            "type": "cppdbg",
            "request": "launch",
            "program": "${workspaceFolder}/Builds/LinuxMakefile/build/MyGUI",
            "args": [],
            "stopAtEntry": false,
            "cwd": "${workspaceFolder}",
            "environment": [],
            "externalConsole": true,
            "MIMode": "gdb",
            "preLaunchTask": "build",
            "setupCommands": [
                {
                    "description": "Enable pretty-printing for gdb",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }
            ]
        }
    ]
}
```