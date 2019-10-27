---
title: ROS-SLAM和move_base仿真地图替换笔记
date: 2019-02-18
categories:
- 笔记
tags:
- 硕论
- ros
toc: true
img: /medias/paperimg/ros.jpg

---
创建自己的地图来进行slam和move_base仿真。<!-- more -->

# 一、slam预置虚拟地图替换
## 1.1 turtlebot
### 1.1.1 编辑地图

**打开gazebo**

```
gazebo
```
**打开编辑器**

快捷键Ctrl+B打开编辑器

**绘制地图**

[参考链接](https://blog.csdn.net/wilylcyu/article/details/51754495)

建议起点坐标（-2，-4）;

根据数据中心的CAD地图，模拟地图绘制为：

长X宽=23mX21m=483平方米；


**保存地图**

将地图保存为DataCenter，放在桌面，注意，一旦退出后，地图不可再编辑，如果不对，只能重建了。
### 1.1.2替换原有地图
打开/home/cdqvm/catkin_0215/src/robot_mrobot/mrobot_gazebo/worlds/cloister.world文件，有两个<model name='room_0'>标签，删除第一个room_0 所有（包括<model name='room_0'>和</model >）内容，
```
 <model name='room_0'>...</model >
```

然后删除第二个room_0**之间**的内容
```
 <model name='room_0'>...</model >
```
并将DataCenter/model.sdf文件中 <model name='DataCenter'>...</model >**之间**的内容（不包含model这两行）粘贴到此处。


### 1.1.3 启动slam仿真程序
```
//启动机器人和地图
roslaunch (robot_mrobot/mrobot_gazebo/launch) mrobot_laser_nav_gazebo.launch
//启动slam程序
roslaunch (robot_mrobot/mrobot_navigation/launch) exploring_slam_demo.launch
```
### 1.1.4 开始探索

详见：[ROS-SLAM自主构建地图仿真](https://purethought.cn/2019/01/11/ROS-SLAM%E8%87%AA%E4%B8%BB%E6%9E%84%E5%BB%BA%E5%9C%B0%E5%9B%BE%E4%BB%BF%E7%9C%9F/)

# 二、move_base地图替换
## 2.1 turtlebot

### 2.1.1 导入地图文件
接着边继续。将slam生成的.pgm和.yaml文件导入到**～/catkin_paper/src/robot_mrobot/mrobot_navigation/maps**文件夹中。
### 2.1.2 修改启动文件
打开～/catkin_paper/src/robot_mrobot/mrobot_navigation/launch/fake_nav_cloister_demo.launch文件，修改default参数为刚导入地图参数文件.yaml的文件名。
```
    <!-- 设置地图的配置文件，设置半径，速度等,这里改错了！ -->
    <arg name="map" default="datacenter.yaml" />
```
## 2.2 rbx1

### 2.2.1 导入地图
将SLAM生成的地图，保存在**~/catkin_0215/src/rbx1/rbx1_nav/maps**文件夹内。
### 2.2.2 启用地图
打开~/catkin_0215/src/rbx1/rbx1_nav/launch/fake_move_base_map_with_obstacles.launch文件，将
```
<node name="map_server" pkg="map_server" type="map_server" args="$(find rbx1_nav)/maps/blank_map_with_obstacle.yaml"/>
```
改为
```
<node name="map_server" pkg="map_server" type="map_server" args="$(find rbx1_nav)/maps/新保存的地图名.yaml"/>
```

### 2.2.3 开始move_base路径规划

详见：[ROS-move_base-rbx1](https://purethought.cn/2019/02/04/ROS-move_base-rbx1/)

