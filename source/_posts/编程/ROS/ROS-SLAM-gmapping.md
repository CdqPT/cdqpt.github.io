---
title: ROS-SLAM-gmapping
date: 2019-1-11
categories:
- 笔记
tags:
- ros
toc: true
img: /medias/paperimg/ros.jpg

---
gmapping是最常用和成熟的slam导航算法，gmapping功能包集成了Rao-Blackwellized粒子滤波算法，为开发者隐去了复杂的内部实现。<!-- more -->


**前提：**已下载并编译了相关功能包集，如还未下载，可通过git下载：[https://github.com/huchunxu/ros_exploring.git](https://github.com/huchunxu/ros_exploring.git)
# 一、启动launch文件
```
cd ~/catkin_ws/src/robot_mrobot/mrobot_gazebo/launch
roslaunch mrobot_laser_nav_gazebo.launch
cd ~/catkin_ws/src/robot_mrobot/mrobot_navigation/launch
roslaunch gmapping_demo.launch
```


解析：

第一个是仿真环境和车；

第二个是导航算法。

# 二、启动键盘控制节点
```
cd ~/catkin_ws/src/robot_mrobot/mrobot_teleop/launch
roslaunch mrobot_teleop.launch
```

出现如下错误：

ERROR: cannot launch node of type [mrobot_teleop/mrobot_teleop.py]: can't locate node [mrobot_teleop.py] in package [mrobot_teleop]
No processes to monitor
shutting down processing monitor...
... shutting down processing monitor complete

是因为对文件没有权限，所以解决方式：

```
cd /home/cdq/catkin_ws/src/robot_mrobot/mrobot_teleop/scripts
chmod +x mrobot_teleop.py
```
再运行launch就可以了

# 三、保存地图
探测完地图后还可以保存地图：

```
rosrun map_server map_saver
```

显示如下：

[ INFO] [1547175819.841787667]: Waiting for the map
[ INFO] [1547175820.094541085]: Received a 608 X 480 map @ 0.050 m/pix
[ INFO] [1547175820.094622238]: Writing map occupancy data to map.pgm
[ INFO] [1547175820.102316792, 2377.983000000]: Writing map occupancy data to map.yaml
[ INFO] [1547175820.102432815, 2377.983000000]: Done

 