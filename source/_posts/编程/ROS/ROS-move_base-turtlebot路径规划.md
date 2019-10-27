---
title: ROS-move_base-turtlebot路径规划
date: 2019-1-11
categories:
- 笔记
tags:
- ros
toc: true
summary: move_base仿真流程。
img: /medias/paperimg/ros.jpg

---
三维仿真环境。<!-- more -->

仿真的整体思路，先启动仿真环境，再启动导航功能。
**前提：**已下载并编译了相关功能包集，如还未下载，可通过git下载：[https://github.com/huchunxu/ros_exploring.git](https://github.com/huchunxu/ros_exploring.git)

# 一、安装依赖
如已在[ROS-SLAM自主构建地图仿真](https://purethought.cn/2019/01/11/ROS-SLAM%E8%87%AA%E4%B8%BB%E6%9E%84%E5%BB%BA%E5%9C%B0%E5%9B%BE%E4%BB%BF%E7%9C%9F/)中安装过依赖，则此步可跳过；

如未在[ROS-SLAM自主构建地图仿真](https://purethought.cn/2019/01/11/ROS-SLAM%E8%87%AA%E4%B8%BB%E6%9E%84%E5%BB%BA%E5%9C%B0%E5%9B%BE%E4%BB%BF%E7%9C%9F/)中安装过依赖，则需要安装第一步的依赖。


# 二、启动仿真环境

```
cd ~/catkin_ws/src/robot_mrobot/mrobot_gazebo/launch
roslaunch mrobot_laser_nav_gazebo.launch
```

# 三、启动路径规划功能

```
cd ~/catkin_ws/src/robot_mrobot/mrobot_navigation/launch
roslaunch fake_nav_cloister_demo.launch
```

#  四、启动随机规划功能

```
rosrun mrobot_navigation random_navigation.py
```

效果如下： 

![image](http://upload-images.jianshu.io/upload_images/16115686-119e7c6c9c5f49fc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们也可以在机器人路径上动态加入一些模型，来测试机器人的导航功能。
