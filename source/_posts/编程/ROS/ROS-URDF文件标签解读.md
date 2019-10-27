---
title: ROS-URDF文件标签解读
date: 2019-1-7
categories:
- 笔记
tags:
- ros
toc: true
img: /medias/paperimg/ros.jpg

---
URDF文件标签解读。<!-- more -->
# 一、连杆（link）标签
|标签|	功能|
|:---:|:---:|
|<link>|	连杆的可视化、碰撞和惯性信息设置|
|<collision>|	设置连杆的碰撞计算的信息|
|<visual>	|设置连杆的可视化信息|
|<inertial>|	设置连杆的惯性信息|
|<mass>|	连杆重量（单位：kg）的设置|
|<inertia>|	惯性张量（Inertia tensor）设置|
|<origin>|	设置相对于连杆相对坐标系的移动和旋转|
|<geometry>	| 输入模型的形状。提供box、cylinder、sphere等形态，也可以导入COLLADA（.dae）、STL（.stl）格式的设计文件。在<collision>标签中，可以指定为简单的形态来减少计算时间|
|<material>|	设置连杆的颜色和纹理|
# 二、关节（joint）标签
|标签|	功能|
|:---:|:---:|
|<joint>|	与连杆的关系和关节类型的设置|
|<parent> |	关节的父连杆 |
|<child> |	关节的子连杆 |
|<origin> 	|将父连杆坐标系转换为子连杆坐标系 |
| <axis>	|设置旋转轴 |
|<limit> |	设置关节的速度、力和半径（仅当关节是revolute或prismatic时）|
|continuous|	旋转关节，可以绕单轴无线旋转|
|revolute|　	旋转关节，类似于continuous，但旋转角度有限|
|prismatic	|滑动关节，沿某一轴线移动的关节，带有位置极限|
|planar	|平面关节，允许在平面正交方向上平移或者旋转|
|floating|	浮动关节，允许进行平移、旋转运动|
|fiexd	|固定关节，不允许运动的特殊关节|
|calibration|	关节参考位置，用来校准关节的绝对位置|
|dynamics	|用于描述物理属性，例如阻尼值、静摩擦力等|
|limit	|用于描述运动的极限值，包括关节的上下限位，速度限制、力矩限制等|
|mimic|	用于描述该关节与已有关节的关系|
|safety-controller|	用于描述安全控制器参数|
#  三、transmission标签
<transmission>是与ROS-CONTROL一起运行所必须的标签，它输入关节与舵机之间的命令接口。

|标签|	功能|
|:---:|:---:|
|<transmission>	|设置关节和舵机之间的变量|
|<type>	|设置力的传递方式的形状|
|<joint>|	设置关节信息设置|
|<hardwareInterface>|	设置硬件接口|
|<actuator>|	设置舵机信息|
|<mechanicalReduction>	|设置舵机与关节之间的齿轮比|