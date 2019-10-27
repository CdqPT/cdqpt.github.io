---
title: ROS-USB摄像头
date: 2019-1-3
categories:
- 笔记
tags:
- ros
toc: true
img: /medias/paperimg/ros.jpg

---
演示使用usb摄像头功能，推荐使用方法二。<!-- more -->
首先要有一个usb摄像头，本次使用的是罗技（Logitech)摄像头。

![image](http://upload-images.jianshu.io/upload_images/16115686-2b9977c01762aa3b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 一、使用软件库里的uvc-camera功能包

## 1.1 检查摄像头

```
lsusb
```


显示如下：

Bus 002 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 001 Device 007: ID 046d:082b Logitech, Inc. Webcam C170
Bus 001 Device 006: ID 0461:4e2a Primax Electronics, Ltd 
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub

## 1.2 安装uvc camera功能包

```
sudo apt-get install ros-kinetic-uvc-camera
```

## 1.3 安装image相关功能包

```
sudo apt-get install ros-kinetic-image-*
sudo apt-get install ros-kinetic-rqt-image-view
```

## 1.4 运行uvc_camera节点

```
rosrun uvc_camera uvc_camera_node
```

## 1.5 查看图像信息

**使用image_view节点查看图像**

```
rosrun image_view image_view image:=/image_raw
```


说明：

最后面的附加选项“image:=/image_raw”是把话题列表中的话题以图像形式查看的选项。

![image](http://upload-images.jianshu.io/upload_images/16115686-eb0ece9ee8687f9d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**用rqt_image_view节点检查**

```
rqt_image_view image:=/image_raw
```

**使用rviz查看**

```
rviz
```

增加image，然后将[Image] → [Image Topic]的值更改为“/image_raw”。

使用apt-get安装的软件包好像只有执行程序，没有launch文件和节点源文件等等，所以采用了自建uvc-camera软件包更该参数。

#  二、使用usb_cam软件包

## 2.1 安装usb_cam软件包

```
sudo apt-get install ros-kinetic-usb-cam
```

##  2.2 启用launch文件

```
roslaunch usb_cam usb_cam-test.launch
```



显示如下： 

![image](http://upload-images.jianshu.io/upload_images/16115686-6e187ddfb853a8eb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

launch文件的目录为：

/opt/ros/kinetic/share/usb_cam

可在该目录下找到luanch文件并修改参数。
