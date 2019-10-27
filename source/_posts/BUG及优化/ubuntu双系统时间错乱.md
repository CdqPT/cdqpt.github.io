
---
title: ubuntu双系统时间错乱
author: 陈德强
date: 2019-02-28 23:40:00
categories:
- bug
tags:
- linux
toc: true
top: false
img: /medias/paperimg/bug.jpg
summary:  解决双系统下时间错乱问题。
---

先在ubuntu下更新一下时间，确保时间无误：
```
sudo apt-get update
sudo apt-get install ntpdate
sudo ntpdate time.windows.com
```
然后将时间更新到硬件上：
sudo hwclock --localtime --systohc

重新进入windows10，发现时间恢复正常了！


参考自：[https://www.jianshu.com/p/9f0b01505385](https://www.jianshu.com/p/9f0b01505385)

