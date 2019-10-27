---
title: ROS-OPENCV
date: 2019-1-11
categories:
- 笔记
tags:
- ros
toc: true
img: /medias/paperimg/ros.jpg

---
opencv是一个开源的跨平台计算机视觉库。<!-- more -->
**前提：**

1.已下载并编译了相关功能包集，如还未下载，可通过git下载：[https://github.com/huchunxu/ros_exploring.git](https://github.com/huchunxu/ros_exploring.git)

2.安装了usb摄像头驱动，如还未安装，可参考：[https://www.cnblogs.com/chendeqiang/p/10217099.html](https://www.cnblogs.com/chendeqiang/p/10217099.html)

# 一、安装opencv
```
sudo apt-get install ros-kinetic-vision-opencv libopencv-dev python-opencv
```

# 二、在ROS中使用opencv

cv_bridge是一种图片格式转换的中间商，可以将ros中的图像格式转化为opencv中的格式。

演示内容：从摄像头获取图像，转化成opencv格式显示，再转化成ros图像格式显示。

## 2.1 单独编译<<ROS机器人开发实践>>  /robot_perception/robot_vision功能包

## 2.2 启动例程

```
roslaunch robot_vision usb_cam.launch
rosrun robot_vision cv_bridge_test.py
rqt_image_view
```

效果如下:

![image](http://upload-images.jianshu.io/upload_images/16115686-ef11d499437f75c6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
