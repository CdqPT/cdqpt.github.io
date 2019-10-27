---
title: ROS-SLAM自主构建地图仿真
date: 2019-1-11
categories:
- 笔记
tags:
- ros
toc: true
img: /medias/paperimg/ros.jpg

---
仿真的整体思路，先启动仿真环境，再启动导航功能。<!-- more -->
**前提：**已下载并编译了相关功能包集，如还未下载，可通过git下载：[https://github.com/huchunxu/ros_exploring.git](https://github.com/huchunxu/ros_exploring.git)

# 一、安装依赖
不安装依赖启动不了个mapping节点。
```
sudo apt-get install ros-kinetic-joy ros-kinetic-teleop-twist-joy ros-kinetic-teleop-twist-keyboard ros-kinetic-laser-proc ros-kinetic-rgbd-launch ros-kinetic-depthimage-to-laserscan ros-kinetic-rosserial-arduino ros-kinetic-rosserial-python ros-kinetic-rosserial-server ros-kinetic-rosserial-client ros-kinetic-rosserial-msgs ros-kinetic-amcl ros-kinetic-map-server ros-kinetic-move-base ros-kinetic-urdf ros-kinetic-xacro ros-kinetic-compressed-image-transport ros-kinetic-rqt-image-view ros-kinetic-gmapping ros-kinetic-navigation

```

# 二、启动仿真环境
仿真环境包括车体模型和地图模型两部分。

```
cd ~/catkin_ws/src/robot_mrobot/mrobot_gazebo/launch
roslaunch mrobot_laser_nav_gazebo.launch
```

# 三、启动slam导航
采用个mapping算法。

```
cd ~/catkin_ws/src/robot_mrobot/mrobot_navigation/launch
roslaunch exploring_slam_demo.launch
```


# 四、开始探索
## 4.1 指定目的地手动探索

使用rviz的“2D nav goal”手动选择目的地。

效果如下:

![image](http://upload-images.jianshu.io/upload_images/16115686-4b7e69009173b862.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如果多次尝试无果，机器人最终会放弃，终端里将看到错误提示。


## 4.2 键盘遥控手动探索

```
 rosrun teleop_twist_keyboard teleop_twist_keyboard.py
```

## 4.3 自动探索
```
rosrun mrobot_navigation random_navigation.py
```
显示如下：

![image](http://upload-images.jianshu.io/upload_images/16115686-0dd106631d045382.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 五、保存地图
```
rosrun map_server map_saver -f map
```
默认保存在home页，一个.pgm文件和一个.yaml文件。


