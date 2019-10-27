---
title: ROS-Rviz-turtlebot3仿真
date: 2019-1-6
categories:
- 笔记
tags:
- ros
toc: true
img: /medias/paperimg/ros.jpg

---
Rviz是ROS自带的一种3D可视化工具。<!-- more -->

# 一、安装turtlebot3功能包

## 1.1 安装依赖包

```
sudo apt-get install ros-kinetic-joy ros-kinetic-teleop-twist-joy ros-kinetic-teleop-twist-keyboard ros-kinetic-laser-proc ros-kinetic-rgbd-launch ros-kinetic-depthimage-to-laserscan ros-kinetic-rosserial-arduino ros-kinetic-rosserial-python ros-kinetic-rosserial-server ros-kinetic-rosserial-client ros-kinetic-rosserial-msgs ros-kinetic-amcl ros-kinetic-map-server ros-kinetic-move-base ros-kinetic-urdf ros-kinetic-xacro ros-kinetic-compressed-image-transport ros-kinetic-rqt-image-view ros-kinetic-gmapping ros-kinetic-navigation
```

## 1.2 安装功能包

```
cd ~/catkin_ws/src/
git clone https://github.com/ROBOTIS-GIT/turtlebot3.git
git clone https://github.com/ROBOTIS-GIT/turtlebot3_msgs.git
git clone https://github.com/ROBOTIS-GIT/turtlebot3_simulations.git
cd ~/catkin_ws && catkin_make
```

# 二、仿真并查看

## 2.1 加载三维模型

```
cd /home/cdq/catkin_ws/src/turtlebot3_simulations/turtlebot3_fake/launch
export TURTLEBOT3_MODEL=burger
roslaunch turtlebot3_fake.launch
```


解析：

第二条是设置环境变量。

第三条运行了tur tlebot3_fake_node节点（三维模型）和robot_state_publisher节点（发布车轮信息）。

显示如下：

![image](http://upload-images.jianshu.io/upload_images/16115686-65b6cf27c7c03a95.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 2.2 开启控制命令 

启用命令后可以用wasd控制方向。

```
cd ~/catkin_ws/src/turtlebot3/turtlebot3_teleop/launch
export TURTLEBOT3_MODEL=burger
roslaunch turtlebot3_teleop_key.launch
```


命令解析：

![image](http://upload-images.jianshu.io/upload_images/16115686-54bc71f8ca8d3416.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

显示如下：

![image](http://upload-images.jianshu.io/upload_images/16115686-10a7d86a900a2c29.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##  2.3 添加里程计信息

```
ADD->By Topic->Odometry
```

初始箭头很大，按图调整一下：

![image](http://upload-images.jianshu.io/upload_images/16115686-48d54e8b76c2f465.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

ADD->TF

![image](http://upload-images.jianshu.io/upload_images/16115686-b6046dd5f9404a0e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

通过rqt_tf_tree查看tf话题

```
rosrun rqt_tf_tree rqt_tf_tree 
```

显示如下：

![image](http://upload-images.jianshu.io/upload_images/16115686-b4fb5fa98c0837d6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
