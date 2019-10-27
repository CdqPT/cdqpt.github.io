---
title: Linux添加快捷方式
date: 2019-04-17 13:36:00
categories: 优化
tags: linux
toc: true
img: /medias/paperimg/bug.jpg
summary: Linux添加快捷方式
author: 陈德强
top: 
password: 
cover: 
---

新建文件名，以.desktop结尾，如pycharm.desktop

```
[Desktop Entry]
Encoding=UTF-8
Name=pycharm
Exec=sh /home/perry/Downloads/pycharm/bin/pycharm.sh    
Icon= /home/perry/Downloads/pycharm/bin/pycharm.png
Info="Spark"
Terminal=false
Type=Application
StartupNotify=true
```

注意Exec和Icon的路径换成软件的路径