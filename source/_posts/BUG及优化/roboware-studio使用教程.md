---
title: roboware-studio使用教程
date: 2017-11-13
categories:
- 教程
tags:
- ros
toc: true
img: /medias/paperimg/bug.jpg
---
roboware是一种针对ros开发的IED集成开发环境，可以大量化简编译流程。优点如下：<!--more-->

1.  从创建工作空间到运行节点整个过程只需要编译一次；

2.  主题，消息和服务等功能对应的文件自动修改；

3.  提供代码索引和自动补充功能；

4.  集成的终端环境等等。

可以到roboware[官网](http://www.roboware.me/)下载软件。

# 一、创建工作区

## 1.1 新建工作区

![image](http://upload-images.jianshu.io/upload_images/16115686-903b706596bdc6da.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 1.2 选择路径并添加工作区的名字 catkin_ws

![image](http://upload-images.jianshu.io/upload_images/16115686-0ffad925b1b9f613.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 二、创建程序包

创建ROS包并添加依赖 my_package roscpp std_msgs

![image](http://upload-images.jianshu.io/upload_images/16115686-3ada3976f5de29c9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 三、添加并编写.cpp源文件

![image](http://upload-images.jianshu.io/upload_images/16115686-df899438ca17a68f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

---------------------------------

![image](http://upload-images.jianshu.io/upload_images/16115686-3b482cb425b4d793.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 四、编译文件

 点击左上角的小锤子,进行编译文件.

![image](http://upload-images.jianshu.io/upload_images/16115686-fc96bf67aabccc31.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
