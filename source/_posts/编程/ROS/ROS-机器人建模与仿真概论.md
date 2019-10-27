---
title: ROS-机器人建模与仿真概论
date: 2019-1-10
categories:
- 笔记
tags:
- ros
toc: true
img: /medias/paperimg/ros.jpg

---
无论是因为高昂的设备费用还是为了减少实验次数，仿真都是十分必要的。<!-- more -->
ROS提供了很多优秀的仿真方式，下面来介绍一下：

URDF：Unified Robot Description Format,统一机器人描述格式。包含对机器人刚体外观、物理属性、关节类型等方面的描述；

xacro：一种可代码复用的机器人描述格式，可精简urdf的代码量；

<gazebo>标签：实现传感器、传动机构等环节的仿真功能；

Rviz：ros自带的查看显示视图及消息的一种可视化软件；

Rviz+ArbotiX：可实现机器人运动控制等功能的仿真；

Gazebo：一种物理仿真软件，可仿真物理环境及传感器。