---
title: ROS-launch-查看日志
date: 2019-1-27
categories:
- 笔记
tags:
- ros
toc: true
img: /medias/paperimg/ros.jpg

---
launch文件的作用是一次可以启动多个节点。<!-- more -->

# 一、新建launch文件

在chapter2_tutorials包下新建launch文件夹，并在launch文件夹下新建chapter2.launch文件，添加如下代码：

```
<?xml version="1.0"?>
<launch>
    <node name="example1_a1" pkg="chapter2_tutorials" type="example1_a1"/>
    <node name="example1_b1" pkg="chapter2_tutorials" type="example1_b1"/>
</launch>
```
# 二、执行launch文件

不需要启动roscore，直接执行以下命令，因为launch会自动启动roscore命令：

```
roslaunch chapter2_tutorials chapter2.launch
```

显示如下：

```
SUMMARY
========

PARAMETERS
* /rosdistro: kinetic
* /rosversion: 1.12.14

NODES
/
example1_a1 (chapter2_tutorials/example1_a1)
example1_b1 (chapter2_tutorials/example1_b1)

auto-starting new master
process[master]: started with pid [13881]
ROS_MASTER_URI=http://localhost:11311

setting /run_id to 12ca110e-0d84-11e9-b214-f44d30974765
process[rosout-1]: started with pid [13894]
started core service [/rosout]
process[example1_a1-2]: started with pid [13906]
process[example1_b1-3]: started with pid [13912]
```
# 三、查看信息

还记得节点example1_b1会在屏幕上输出从其他节点收到的信息，现在却看不到了，这是因为example1_b使用ROS_INFO输出信息。当在shell中只运行一个节点时，可以看到它。但是当运行启动文件时，则看不到它。

现在为了看到消息，可以运行rqt_console实用程序。

```
rqt_console
```
显示如下：

![image](http://upload-images.jianshu.io/upload_images/16115686-d9ee3b9cf28e2817.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
