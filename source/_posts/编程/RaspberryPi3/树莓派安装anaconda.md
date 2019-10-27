---
title: 树莓派安装anaconda
date: 2019-05-02 21:14:00
categories: 教程
tags: 嵌入式
toc: true
img: /medias/paperimg/raspi.jpg
summary: 树莓派安装anaconda arm版miniconda
author: 陈德强
top: 
password: 
cover: 
---

首先查看树莓派类型：
```
uname -a
```
Linux raspberrypi 4.14.98-v7+ #1200 SMP Tue Feb 12 20:27:48 GMT 2019 armv7l GNU/Linux
查看到是armv7架构，
跳转到网址[https://repo.continuum.io/miniconda/](https://repo.continuum.io/miniconda/)下载最新版支持armv7架构的安装包，例如我下的是2015年8月24日的  `Miniconda-3.16.0-Linux-armv7l.sh`
或者直接使用命令安装：
```
wget https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-armv7l.sh
```
```
下载好后安装miniconda
```
cd ~/Downloads
```
```
sudo bash Miniconda3-latest-Linux-armv7l.sh
```
enter
yes
就安装成功了。

添加环境：
```
sudo nano /home/pi/.bashrc # -> add: export PATH="/home/pi/miniconda3/bin:$PATH"
```
ctrl+x
然后测试一下：
```
python
```
就会显示python3.4了

参考文献：
1. [Raspbian Miniconda安装配置](https://www.jianshu.com/p/24931aa48855)
