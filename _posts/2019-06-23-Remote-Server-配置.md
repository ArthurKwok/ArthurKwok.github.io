---
layout:     post
title:      Remote Server 配置
subtitle:   
date:       2020-07-27
author:     Arthur
header-style: text
catalog: true
tags: Linux

---
## 更新
2020-07-27：配置更新至Docker（含jupyter lab）+code-server+ngrok

### Docker指令
```
sudo docker run -it --gpus all --rm --name mir -e JUPYTER_ENABLE_LAB=yes --mount type=bind,source=/mnt/d/ubuntu/Workspace,target=/home/jovyan/workspace -p 8888:8888 -p 6006:6006 arthurgjy/mir-aio:latest-gpu
```

### code-server指令
```
code-server
### config path
### ~/.config/code-server/config.yaml
```

### ngrok指令
```
cd Dowloads
./ngrok start --all --region ap
### config path
### ~/.ngrok2/ngrok.yml
```

### 端口与域名
```
http://g××××h.code.ap.ngrok.io -> http://localhost:8080                    
https://g××××h.code.ap.ngrok.io -> http://localhost:8080                   
http://g××××h.lab.ap.ngrok.io -> http://localhost:6006                     
https://g××××h.lab.ap.ngrok.io -> http://localhost:6006                    
http://g××××h.monitor.ap.ngrok.io -> http://localhost:4040                 
https://g××××h.monitor.ap.ngrok.io -> http://localhost:4040
```

============================================ 分割线 ==============================================
=================================================================================================

## 2019-06-23
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

