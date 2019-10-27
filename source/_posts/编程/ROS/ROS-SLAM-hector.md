---
title: ROS-SLAM-hector
date: 2019-1-11
categories:
- 笔记
tags:
- ros
toc: true
img: /medias/paperimg/ros.jpg

---
hector_slam可以很好的在空中机器人，手持构图设备及特种机器人中运行。

hector_slam不需要订阅里程计信息/odmo消息，而是直接使用激光估算里程计信息，因此，当机器人速度较快时会发生打滑现象，导致建图效果出现误差。<!-- more -->




**前提：**已下载并编译了相关功能包集，如还未下载，可通过git下载：[https://github.com/huchunxu/ros_exploring.git](https://github.com/huchunxu/ros_exploring.git)

# 一、安装hector_slam功能包

ros中已集成了hector_slam功能包

```
sudo apt-get install ros-kinetic-hector-slam
 ```

# 二、在Gzebo中仿真SLAM

## 2.1 启动仿真环境

```
cd ~/catkin_ws/src/robot_mrobot/mrobot_gazebo/launch
roslaunch mrobot_laser_nav_gazebo.launch
```

## 2.2 启动slam导航

```
cd ~/catkin_ws/src/robot_mrobot/mrobot_navigation/launch
roslaunch hector_demo.launch
```

## 2.3 启动键盘控制

```
roslaunch mrobot_teleop mrobot_teleop.launch 
```


显示如下：

![image](http://upload-images.jianshu.io/upload_images/16115686-59e79c5606ce1a76.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
