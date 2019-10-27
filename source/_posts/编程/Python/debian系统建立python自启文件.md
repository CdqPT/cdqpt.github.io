---
title: debian系统建立python自启文件
date: 2019-04-29 12:03:00
categories: 优化
tags: linux
toc: true
img: /medias/paperimg/python.jpg
summary: debian系统建立python开机自启文件。
author: 陈德强
top: 
password: 
cover: 
---

这种方式必须把文件放在桌面才可以！！！！！

# 一、新建.sh文件
在桌面新建test.sh文件，里面加上要自启文件的路径和内容，例如：
```
cd /home/pi/Desktop
python3 gpio.py
```

# 二、启用启动文件
进入`home/pi/.config/autostart`
输入以下信息：
```
[Desktop Entry]
Name=python_startup
Comment=Python Program
Exec=sh /home/pi/Desktop/test.sh
Terminal=true
MultipleArgs=false
Type=Application
Categories=Application;Development;
StartupNotify=true
```
其中`Exec=sh /home/pi/Desktop/test.sh`就是需要启动的文件。