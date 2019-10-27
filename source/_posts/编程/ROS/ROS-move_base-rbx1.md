---
title: ROS-move_base-rbx1
date: 2019-02-04
categories:
- 笔记
tags:
- ros
toc: true
img: /medias/paperimg/ros.jpg

---

slam可以自主构建地图，而move_base则可以计算最短路径进行路径规划。<!-- more -->


>基本流程：
```
roslaunch （rbx1/rbx1_bringup/launch） fake_turtlebot.launch 
roslaunch （rbx1/rbx1_nav/launch）fake_move_base_map_with_obstacles.launch
rosrun rviz rviz -d `rospack find rbx1_nav`/nav.rviz
```


# 一、下载并编译rbx1功能包
rbx1功能包包含了路径规划的算法和功能，但不具有机器人模型。
```
git clone https://github.com/pirobot/rbx1.git
```
# 二、下载并编译arbotix 功能包
arbotix 功能包是仿真器，包含车的仿真模型。
```
sudo apt-get install ros-kinetic-arbotix*
```
如果命令安装不能用，也可以使用源码安装，源码地址：
```
git clone https://github.com/vanadiumlabs/arbotix_ros.git
```
# 三、本地导航功能包简介
本地的实时规划是利用base_local_planner包实现的。该包使用Trajectory Rollout 和Dynamic Window approaches算法计算机器人每个周期内应该行驶的速度和角度（dx，dy，dtheta velocities）。

通过地图数据，通过算法搜索到达目标的多条路经，利用一些评价标准（是否会撞击障碍物，所需要的时间等等）选取最优的路径，并且计算所需要的实时速度和角度。

其中，Trajectory Rollout 和Dynamic Window approaches算法的主要思路如下：
      （1） 采样机器人当前的状态（dx,dy,dtheta）；
      （2） 针对每个采样的速度，计算机器人以该速度行驶一段时间后的状态，得出一条行驶的路线。
      （3） 利用一些评价标准为多条路线打分。
      （4） 根据打分，选择最优路径。
      （5） 重复上面过程。
# 四、手动规划
## 4.1 启动模型
首先运行ArbotiX节点，加载机器人模型。
```
roslaunch rbx1_bringup fake_turtlebot.launch 
```
此时还没有启动rviz，所以看不见地图

显示如下：

------------------------------
setting /run_id to 0e92025a-2854-11e9-b3d0-000c29736726
process[rosout-1]: started with pid [20986]
started core service [/rosout]
process[arbotix-2]: started with pid [20991]
process[robot_state_publisher-3]: started with pid [21004]
[INFO] [1549267712.477219]: ArbotiX being simulated.
[INFO] [1549267712.509029]: Started DiffController (base_controller). Geometry: 0.26m wide, 4100.0 ticks/m.

------------------------------

## 4.2 加载空白地图
```
  roslaunch rbx1_nav fake_move_base_blank_map.launch 
```
显示如下:

-----------------------------------
[ INFO] [1549267889.458808264]: Sim period is set to 0.33
[ INFO] [1549267890.102050652]: Recovery behavior will clear layer obstacles
[ INFO] [1549267890.110597355]: Recovery behavior will clear layer obstacles
[ INFO] [1549267890.199764630]: odom received!

-------------------------------------------

## 4.3 启动RVIZ
```
rosrun rviz rviz -d `rospack find rbx1_nav`/nav.rviz
```
![image.png](https://upload-images.jianshu.io/upload_images/16115686-30122d39990e6447.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

点击rviz上方的**2D Nav Goal**按键，选择适当位置和方向后就可以导航了。


![image.png](https://upload-images.jianshu.io/upload_images/16115686-cef51c46ffb635eb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如果不想看这些红箭头，可以勾选左侧Odometry下的**Unreliable**



# 五、自动规划
## 5.1 空白地图自动导航
走正方形
```
rosrun rbx1_nav move_base_square.py 
```
![image.png](https://upload-images.jianshu.io/upload_images/16115686-ba110ca6a0cb250f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
## 5.2 障碍地图自动导航

### 5.2.1 关掉fake_move_base_blank_map.launch

### 5.2.2 启动带障碍的地图
```
roslaunch fake_move_base_map_with_obstacles.launch
```
![image.png](https://upload-images.jianshu.io/upload_images/16115686-c1fdb7ac1b31c26a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
障碍地图可以自建，参考：[ROS-SLAM和move_base仿真地图替换笔记 ](https://purethought.cn/2019/02/18/ROS-SLAM%E5%92%8Cmove_base%E4%BB%BF%E7%9C%9F%E5%9C%B0%E5%9B%BE%E6%9B%BF%E6%8D%A2%E7%AC%94%E8%AE%B0/)

## 5.3 走正方形
```
rosrun rbx1_nav move_base_square.py   
```
![image.png](https://upload-images.jianshu.io/upload_images/16115686-f7ec740fe59c154e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**修改rbx1_nav move_base_square.py 文件**
打开**～/catkin_paper/src/rbx1/rbx1_nav/nodes/move_base_square.py**文件，修改需要到达的坐标点。
```
 waypoints.append(Pose(Point(3.0, square_size, 0.0), quaternions[0]))
waypoints.append(Pose(Point(3.0, 0.0, 0.0), quaternions[1]))
waypoints.append(Pose(Point(0.0, 0.0, 0.0), quaternions[2]))
waypoints.append(Pose(Point(0.0, square_size, 0.0), quaternions[3]))
```
其中，Pose（Point，quaternions）的两个参数分别为位姿和点编号；

Point（x，y，？）的三个参数分别为横坐标，纵坐标和方向？


参考自：[http://www.guyuehome.com/270](http://www.guyuehome.com/270)