---
title: 双系统Ubuntu无法访问Windows磁盘分区解决方法
date: 2017-11-12
categories:
- bug
tags:
- linux
toc: true
img: /medias/paperimg/bug.jpg
---


为了更好的体验各种操作系统，在电脑中安装双系统是很好的选择，但在使用中难免会遇到这样或那样的问题.<!--more-->

最近总是遇到[Ubuntu](http://www.linuxidc.com/topicnews.aspx?tid=2 "Ubuntu")系统下无法访问Windows磁盘分区问题，看了系统日志发现是挂载磁盘出问题了，解决方法是使用ntfs来修复后重新挂载。

# 一、安装ntfs

```
sudo apt-get install ntfs-3g
```

# 二、修复挂载

首先查看不能访问的磁盘的分区号：

一般点击对应磁盘会提示是哪一个分区， 比如我的问题出现在 sda6，当然也可以用fdisk -l 查看磁盘信息.

以sda6为例，修复命令为：

```
sudo ntfsfix /dev/sda6
```

![image](http://upload-images.jianshu.io/upload_images/16115686-6e6b97e727027ac3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 三、访问磁盘

![image](http://upload-images.jianshu.io/upload_images/16115686-16e475a1809e9e16.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

