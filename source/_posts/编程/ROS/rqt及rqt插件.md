---
title: rqt及rqt插件
author: 陈德强
date: 2019-03-08 10:30:00
categories:
- 笔记
tags:
- ros
toc: true
top: false
summary:  rqt及rqt插件。
img: /medias/paperimg/ros.jpg

---
# 一、ROS可视化工具rqt
## 1.1 安装rqt
当然一般情况下，安装ros都会自带rqt，这里仅作为补充。
```
sudo apt-get install ros-kinetic-rqt*
```
## 1.2 运行rqt
```
rqt
```
![image](https://wx1.sinaimg.cn/large/006jQClrly1g0v77qcfn5j30zj0kvwmf.jpg)
正常启动时是最小化状态，菜单栏可以在最上方的导航栏看到。rqt的工具有很多，包括：动作、配置、自检、日志、插件、机器人工具、服务、话题、可视化等。
## 1.3  rqt_image_view
这是一个显示相机的图像数据的插件。
运行命令
```
rqt_image_view
```
![rqt_image_view](https://wx1.sinaimg.cn/large/006jQClrly1g0v7fay83fj30n80ks1a3.jpg)
## 1.4  rqt_graph
rqt_graph是用图形表示当前活动中的节点与在ROS网络上传输的消息之间的相关
性的工具，这对了解当前ROS网络情况非常有用。
```
rqt_graph
```
![image](https://wx4.sinaimg.cn/large/006jQClrly1g0v7g4xm40j30t00e9ae4.jpg)
## 1.5  rqt_plot
rqt_plot是一个二维数据绘图工具。绘图意味着绘制坐标。换句话说，它接收到
ROS消息并将其绘制在坐标系上。
```
rqt_plot
```
### 举个栗子：

**启动小乌龟程序**
```
 rosrun turtlesim turtlesim_node
```
**启动键盘控制**
```
rosrun turtlesim turtle_teleop_key
```
**启动rqt_plot**
```
rqt_plot
```
删掉topic中的斜杠"/"

![image](https://wx3.sinaimg.cn/large/006jQClrly1g0v7p4cbruj313r0kvk0k.jpg)

再重新输入一遍斜杠"/"就会出现所有可以显示的话题

![image](https://wx1.sinaimg.cn/large/006jQClrly1g0v7saq9zuj311d0k4gty.jpg)

选择turtle1/pose，然后点击加号“+”，就会显示出当前可显示的曲线。

![image.png](https://upload-images.jianshu.io/upload_images/16115686-172656022de4bb71.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

此时控制键盘会显示变化的曲线

![image](https://wx2.sinaimg.cn/large/006jQClrly1g0v7xz08kjj31360nrqc1.jpg)

## 1.6 rqt_bag
rqt_bag是一个可以将消息进行可视化的GUI工具。可以立即查看摄像机的图像值，即回放功能。

详见<<ROS机器人编程>>