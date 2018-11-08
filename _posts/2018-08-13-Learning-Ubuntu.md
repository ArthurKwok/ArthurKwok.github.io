---
layout:     post
title:      Learning Ubuntu
subtitle:   
date:       2018-08-13
author:     Arthur
header-style: text
catalog: true
tags:
    - Linux
---

安装ubuntu三天也算是跳了很多坑。。记录一下一些常用的操作，方便以后来查找


##基本环境

###安装linux

1. 清理出一个100g的硬盘空间给新系统使用，在windows的磁盘管理中格式化了原来黑苹果的OSX硬盘

2. 在ubuntu官网下载ubuntu 18.04 LTS 
    https://www.ubuntu.com/download/
3. 制作引导u盘
使用软件：```ultraISO```

4. F12启动u盘 boost,依照程序安装；选择minimal install和附加驱动

5. 安装成功后，首先验证wifi驱动，之后切换到阿里云repository。

###Shadowsocks

科学上网分为两个部分，首先要安装shadowsocks软件。
最简单的方法可以直接在应用商店搜索shadowscoks下载shadoqsocks-qt5的个人版本，按照服务器配置即可。也可以使用 ```apt-get```:

    sudo add-apt-repository ppa:hzwhuang/ss-qt5
    sudo apt-get update
    sudo apt-get install shadowsocks-qt5
记得打开自动连接喔

之后要配置本地软件的端口。可以选择仅仅将浏览器进行配置（switchyomega），也可以选择配置全局PAC。
PAC的方法参考使用genPAC：
https://www.litcc.com/2016/12/29/Ubuntu16-shadowsocks-pac/index.html

配置路径：
file:///home/arthur/autoproxy.pac

### ShadowsocksR

github page: https://github.com/erguotou520/electron-ssr/releases

##系统美化

美化主要参考教程：
https://zhuanlan.zhihu.com/p/36200924
https://zhuanlan.zhihu.com/p/37314255

###安装Gnome-tweak-tool

    sudo apt-get install gnome-tweak-tool

安装插件：

  - User theme
  - Dash to dock
  - Alternatetab
  - Hide top bar
  - No workspace switcher popup

调整字体比例，触摸板速度等等

###安装主题、图标

主题：arc

    sudo apt install arc-theme

图标：papirus

    sudo add-apt-repository ppa:papirus/papirus
    sudo apt-get update
    sudo apt-get install papirus-icon-theme

### Boot loader

使用 `rEFInd`:
```
$ sudo apt-add-repository ppa:rodsmith/refind
$ sudo apt-get update
$ sudo apt-get install refind
```
安装主题：
挂载EFI引导盘：
```
$ sudo nautilus
``` 
在目录`efi/EFI/refind`中新建文件夹`themes`,下载并拷贝：
```https://github.com/EvanPurkhiser/rEFInd-minimal```
在`rEFInd`目录中，`refind.conf`最后一行添加：
```include themes/rEFInd-minimal/theme.conf```

### 替换方向键

参考：https://shellhell.wordpress.com/2012/01/31/hello-world/

## 软件安装

### Opencv

http://www.codebind.com/cpp-tutorial/install-opencv-ubuntu-cpp/

https://blog.csdn.net/zoeou/article/details/80934367

### Chrome

使用自带的firefox（对不起）搜索chrome，可在未翻墙状态下下载。
之后用google账号可同步所有的设置、浏览记录。

### Matlab

matlab的安装相当之蛋疼。。
首先，在mathworks官网登陆以后即可下载linux版本matlab；
然后忍受纳米级别的字体。。在安装完成以后才有可能更换。
在安装的过程中，记得去除simulink包，容量巨大。
安装完成之后马上进入激活环节：记得一定要用自己本机的用户名输入！否则license会有问题。
此时还暂时没有图标，使用命令行进入：

    /usr/local/MATLAB/R2018a/bin/matlab

切记不可加sudo！否则会提示用户名不匹配
进入后字体小到瞎眼！在matlab terminal 中 更改scaling facor：

    >> s = settings;s.matlab.desktop.DisplayScaleFactor
    >> s.matlab.desktop.DisplayScaleFactor.PersonalValue = 1.5
    
在应用商店搜索matlab，可添加app图标。

最后切换默认的快捷键：
preferences->keyboard->shortcuts, 将emacs改为windows


### VScode

在微软官网下载即可。

### anaconda

https://blog.csdn.net/SONGYINGXU/article/details/78940305

### Zotero

需要安装两个部分：
zotero for linux
zotero chrome插件

### Office

建议使用web office：

### JUCE

reference: https://mycourses.aalto.fi/pluginfile.php/108835/mod_resource/content/4/juce_instructions.pdf

download JUCE from offical site;
On Linux, before you can compile any JUCE code, you have to install the
following list of packages that JUCE depends on:
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
sudo apt-get -y install libcurl4-gnutls-dev
```
### C++

reference:
https://www.cnblogs.com/lidabo/p/5888997.html
https://blog.csdn.net/q932104843/article/details/51924900
https://www.zhihu.com/question/30315894

C++ coding environment:
```
sudo apt-get install build-essential
gcc --version
sudo apt-get install g++-4.8
```



###

##常用操作

 - 转换目录：
```
    cd /dir
    cd ..
    cd ~/
```

 - 文件操作：
```
    sudo cp
    sudo rm
    ls
```
 - 网络：
```
    sudo hwsl (-C network)
    ifconfig
    iwconfig
```
 - 关机：
```
    sudo shutdown -h now
    sudo reboot
```
 

