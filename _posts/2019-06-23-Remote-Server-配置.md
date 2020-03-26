---
layout:     post
title:      Remote Server 配置
subtitle:   
date:       2019-06-23
author:     Arthur
header-style: text
catalog: true
tags: Linux

---

昨天微软发布了Windows Terminal的预览版本，我第一时间尝鲜试用。为了更好地利用WT，顺便申请了一个服务器进行远程的编程环境配置。现在记录一下配置过程。

使用的服务：
 - 阿里云
 - code-server
 - Windows Terminal
 - Anaconda
### code-server 配置

```bash
# Download
wget https://github.com/cdr/code-server/releases/download/1.1156-vsc1.33.1/code-server1.1156-vsc1.33.1-linux-x64.tar.gz

# express
tar -xzvf code-server1.1156-vsc1.33.1-linux-x64.tar.gz

cd code-server1.1156-vsc1.33.1-linux-x64

mv code-server /bin
```


之后切换到想要作为vscode根目录的位置，输入：

```bash
code-server
```
即可开启服务器。


### Anaconda 配置
须在root账号下运行
```
wget https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/Anaconda3-2019.03-Linux-x86_64.sh
```

