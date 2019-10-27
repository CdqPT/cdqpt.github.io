
---
title: ROS-SLAM和move_base导入自己的机器人模型
date: 2019-02-15
categories:
- 笔记
tags:
- ros
toc: true
img: /medias/paperimg/ros.jpg

---

使用自己的urdf机器人进行slam地图构建和move_base路径规划。<!-- more -->
# 一、turtlebot机器人（mrobot功能包）模型替换
## 1.1 生成机器人模型
详见:[参考链接](https://purethought.cn/2019/01/21/ROS-Solidworks%E8%BD%ACURDF/)

最后导出mycar2功能包。
## 1.2 导入mycar2功能包
将mycar2功能包放入src内，

然后将package.xml文件中的邮箱添加一个@，不然会报错。
```
<maintainer email="me2email.com" />
改成
<maintainer email="me2@email.com" />
```


## 1.3 修改urdf文件

修改robot_mrobot/mrobot_gazebo/urdf/mrobot_body.urdf.xacro文件，将


```html
 <link name="base_link">
            <cylinder_inertial_matrix  m="${base_mass}" r="${base_link_radius}" h="${base_link_length}" />
            <visual>
                <origin xyz=" 0 0 0" rpy="0 0 0" />
                <geometry>
                    <cylinder length="${base_link_length}" radius="${base_link_radius}"/>
                </geometry>
                <material name="yellow" />
            </visual>

            <collision>
                <origin xyz="0 0 0" rpy="0 0 0" />
                <geometry>
                    <cylinder length="${base_link_length}" radius="${base_link_radius}"/>
                </geometry>
            </collision>   
        </link>
```
修改为

```html
 <link name="base_link">
            <cylinder_inertial_matrix  m="${base_mass}" r="${base_link_radius}" h="${base_link_length}" />
            <visual>




                <origin xyz=" 0 0 0" rpy="-1.57 0 1.57" />
                <geometry>
                     <mesh filename="package://mycar2/meshes/base_link.STL" /> 





                </geometry>
                <material name="yellow" />
            </visual>

            <collision>
                <origin xyz="0 0 0" rpy="0 0 0" />
                <geometry>
                    <cylinder length="${base_link_length}" radius="${base_link_radius}"/>
                </geometry>
            </collision>   
        </link>
```

## 1.4 编译

## 1.5 启动仿真流程

```
//启动仿真环境
roslaunch （robot_mrobot/mrobot_gazebo/launch）mrobot_laser_nav_gazebo.launch
//启动手动slam
roslaunch （robot_mrobot/mrobot_navigation/launch）fake_nav_cloister_demo.launch
//启动自动slam
rosrun mrobot_navigation random_navigation.py
```

注：括号内（robot_mrobot/mrobot_gazebo/launch）为文件路径，需要对应寻找，而不能直接复制代码来用。以下除特殊说明，皆是如此。


# 二、rbx1机器人（rbx1功能包）模型替换

## 2.1 载入功能包
将rbx1功能包放入src内。
## 2.2 修改urdf文件
修改rbx1/rbx1_description/urdf/turtlebot_body.urdf.xacro文件，将

```html
 <link name="base_link">
    <inertial>
      <mass value="2" />
      <origin xyz="0 0 0.0" />
      <inertia ixx="0.01" ixy="0.0" ixz="0.0"
        iyy="0.01" iyz="0.0" izz="0.5" />
    </inertial>

    <visual>
      <origin xyz=" 0 0 0.0308" rpy="0 0 0" />
      <geometry>
        <mesh filename="package://rbx1_description/meshes/create_body.dae" scale="${MESH_SCALE} ${MESH_SCALE} ${MESH_SCALE}"/>


        
      </geometry>
    </visual>

    <collision>
      <origin xyz="0.0 0.0 0.0308" rpy="0 0 0" />
      <geometry>
        <cylinder length="0.0611632" radius="0.016495"/>
      </geometry>
    </collision>
  </link>
```
修改为
```html
<link name="base_link">
    <inertial>
      <mass value="2" />
      <origin xyz="0 0 0.0" />
      <inertia ixx="0.01" ixy="0.0" ixz="0.0"
        iyy="0.01" iyz="0.0" izz="0.5" />
    </inertial>




    <visual>
      <origin xyz=" 0 0 0.0308" rpy="-1.57 0 1.57" />
      <geometry>
        <mesh filename="package://mycar2/meshes/base_link.STL" /> 

        


      </geometry>
    </visual>

    <collision>
      <origin xyz="0.0 0.0 0.0308" rpy="0 0 0" />
      <geometry>
        <cylinder length="0.0611632" radius="0.016495"/>
      </geometry>
    </collision>
  </link>
```

## 2.3 编译

## 2.4 启动仿真流程

```
//启动仿真模型
roslaunch （rbx1/rbx1_bringup/launch） fake_turtlebot.launch
//启动虚拟地图+move_base
roslaunch （rbx1/rbx1_nav/launch）fake_move_base_map_with_obstacles.launch
//启动rviz
rosrun rviz rviz -d `rospack find rbx1_nav`/nav.rviz
```

