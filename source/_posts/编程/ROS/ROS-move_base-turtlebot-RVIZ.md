---
title: ROS-move_base-turtlebot-RVIZ
date: 2019-1-11
categories:
- 笔记
tags:
- ros
toc: true
img: /medias/paperimg/ros.jpg

---

二维仿真环境。<!-- more -->

slam使用激光雷达完成了地图构建，现在介绍一下自主导航。move_base用于实现最优路径规划，amcl用于实现机器人定位。
**前提：**已下载并编译了相关功能包集，如还未下载，可通过git下载：[https://github.com/huchunxu/ros_exploring.git](https://github.com/huchunxu/ros_exploring.git)

# 一、安装功能包 
## 1.1 安装导航包

```
sudo apt-get install ros-kinetic-navigation
```
## 1.2 克隆arbotix仿真模型
```
git clone  https://github.com/vanadiumlabs/arbotix_ros.git
```
## 1.3 安装arbotix插件
```
sudo apt-get install ros-kinetic-arbotix*
```
# 二、启动模型文件

```
cd ~/catkin_ws/src/robot_mrobot/mrobot_bringup/launch
roslaunch fake_mrobot_with_laser.launch
```

# 三、启动导航文件

```
cd ~/catkin_ws/src/robot_mrobot/mrobot_navigation/launch
roslaunch fake_nav_demo.launch
```

显示如下：

![image](http://upload-images.jianshu.io/upload_images/16115686-55c4a4e03874557c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 四、手动导航

在rviz界面点击“2D Nav Goal”按钮，这个按钮用于设置导航的目标点；

鼠标左键点击目标点不要松开，选择方向后再松开；

然后机器人就会自动规划路径并导航了。

![image](http://upload-images.jianshu.io/upload_images/16115686-4558c7947ee9ae42.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 五、自动导航

启动自动导航

```
rosrun mrobot_navigation random_navigation.py
```
如果出现报错：

[rosrun] Couldn't find executable named random_navigation.py below /home/cdq/catkin_ws/src/robot_mrobot/mrobot_navigation

这是因为没有权限，解决方式：

```
cd /home/cdq/catkin_ws/src/robot_mrobot/mrobot_navigation/scripts
chmod +x random_navigation.py
```

然后再运行命令就可以了。



随机导航效果如下：

![image](http://upload-images.jianshu.io/upload_images/16115686-66043901fe7ac8a0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 六、查看信息

打开日志监控的可视化终端，可以看到机器人发布的距离信息、状态信息、目标点编号、成功率和速度等日志。 

```
rqt_console
```

效果如下：

![image](http://upload-images.jianshu.io/upload_images/16115686-d6e63f71722407fe.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
