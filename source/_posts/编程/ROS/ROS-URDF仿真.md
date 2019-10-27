---
title: ROS-URDF仿真
date: 2019-1-1
categories:
- 笔记
tags:
- ros
toc: true
img: /medias/paperimg/ros.jpg

---
URDF （标准化机器人描述格式）,是一种用于描述机器人及其部分结构、关节、自由度等的XML格式文件。<!-- more -->
# 一、首先做一个带有四个轮子的机器人底座
## 1.1 新建urdf文件
在chapter4_tutorials/robot1_description/urdf文件夹下新建robot1.urdf文件，内容如下：
```html
<?xml version="1.0"?>
<robot name="robot1">     
    <link name="base_link">
           <visual>
                 <geometry>
                       <box size="0.2 .3 .1"/>
                 </geometry>
            <origin rpy="0 0 0" xyz="0 0 0.05"/>
            <material name="white">
                <color rgba="1 1 1 1"/>
            </material>
           </visual>
        <collision>
            <geometry>
                       <box size="0.2 .3 0.1"/>
            </geometry>
        </collision>
        <inertial>
            <mass value="100"/>
             <inertia ixx="1.0" ixy="0.0" ixz="0.0" iyy="1.0" iyz="0.0" izz="1.0"/>
        </inertial>        
     </link>
     <link name="wheel_1">
           <visual>
                 <geometry>
                       <cylinder length="0.05" radius="0.05"/>
                 </geometry>
            <origin rpy="0 1.5 0" xyz="0.1 0.1 0"/>
               <material name="black">
                <color rgba="0 0 0 1"/>
            </material>
        </visual>
        <collision>
            <geometry>
                       <cylinder length="0.05" radius="0.05"/>
            </geometry>
        </collision>
        <inertial>
            <mass value="10"/>
             <inertia ixx="1.0" ixy="0.0" ixz="0.0" iyy="1.0" iyz="0.0" izz="1.0"/>
        </inertial>
     </link>

     <link name="wheel_2">
           <visual>
                 <geometry>
                       <cylinder length="0.05" radius="0.05"/>
                 </geometry>
            <origin rpy="0 1.5 0" xyz="-0.1 0.1 0"/>
               <material name="black"/>
           </visual>
        <collision>
            <geometry>
                       <cylinder length="0.05" radius="0.05"/>
            </geometry>
        </collision>
        <inertial>
            <mass value="10"/>
             <inertia ixx="1.0" ixy="0.0" ixz="0.0" iyy="1.0" iyz="0.0" izz="1.0"/>
        </inertial>
     </link>
     <link name="wheel_3">
           <visual>
                 <geometry>
                       <cylinder length="0.05" radius="0.05"/>
                 </geometry>
            <origin rpy="0 1.5 0" xyz="0.1 -0.1 0"/>
               <material name="black"/>
           </visual>
        <collision>
            <geometry>
                       <cylinder length="0.05" radius="0.05"/>
            </geometry>
        </collision>
        <inertial>
            <mass value="10"/>
             <inertia ixx="1.0" ixy="0.0" ixz="0.0" iyy="1.0" iyz="0.0" izz="1.0"/>
        </inertial>
     </link>
     <link name="wheel_4">
           <visual>
                 <geometry>
                       <cylinder length="0.05" radius="0.05"/>
                 </geometry>
            <origin rpy="0 1.5 0" xyz="-0.1 -0.1 0"/>
               <material name="black"/>
           </visual>
        <collision>
            <geometry>
               <cylinder length="0.05" radius="0.05"/>
            </geometry>
        </collision>
        <inertial>
            <mass value="10"/>
             <inertia ixx="1.0" ixy="0.0" ixz="0.0" iyy="1.0" iyz="0.0" izz="1.0"/>
        </inertial>
     </link>
     <joint name="base_to_wheel1" type="fixed">
           <parent link="base_link"/>
           <child link="wheel_1"/>
           <origin xyz="0 0 0"/>
     </joint>
     <joint name="base_to_wheel2" type="fixed">
           <parent link="base_link"/>
           <child link="wheel_2"/>
           <origin xyz="0 0 0"/>
     </joint>
     <joint name="base_to_wheel3" type="fixed">
           <parent link="base_link"/>
           <child link="wheel_3"/>
           <origin xyz="0 0 0"/>
     </joint>
     <joint name="base_to_wheel4" type="fixed">
           <parent link="base_link"/>
           <child link="wheel_4"/>
           <origin xyz="0 0 0"/>
     </joint>
</robot>
```
                                                       

----------------------------

解析：urdf文件主要有link和joint组成。 

<robot name="robot1">   机器人标签，机器人名字为robot1                     

<link name="base_link">  连杆标签，连杆名字为base_link

<visual>  可视化属性

<geometry>  形状

<box size="0.2 .3 .1"/>   方形，尺寸为0.2m*0.3m*1m

<origin rpy="0 0 0" xyz="0 0 0.05"/>  起点位置无位移，绕z轴旋转。

<material name="white">  材料名称为白色

<color rgba="1 1 1 1"/>  颜色属性为1 1 1 1 

<collision>  

<inertial>

<mass value="100"/>

<inertia ixx="1.0" ixy="0.0" ixz="0.0" iyy="1.0" iyz="0.0" izz="1.0"/>

<joint name="base_to_wheel1" type="fixed">  joint标签，关节名称为base_to_wheel1，类型为固定。continuous为绕某一轴旋转

<parent link="base_link"/>  父连杆是base_link

<child link="wheel_1"/>子连杆是wheel_1

<origin xyz="0 0 0"/>   起点位置无偏移。注：这里的起点是相对与父连杆的位置，并不是绝对坐标。

---------------------

 为了检查书写的语法是否正确和配置是否有误，可以使用语法检查工具：

```
check_urdf robot1.urdf
```


显示如下：

robot name is: Robot1
---------- Successfully Parsed XML ---------------
root Link: base_link has 4 child(ren)
child(1): wheel1
child(2): wheel2
child(3): wheel3
child(4): wheel4

**查询工具（可选）**

使用urdf_to_graphiz命令工具：

```
urdf_to_graphiz robot1.urdf
```

生成两个文件：robot1.pdf和robot1.gv

直接查看robot1.pdf或者使用命令：

```
evince Robot1.pdf
```


显示如下：

![image](http://upload-images.jianshu.io/upload_images/16115686-149ca0acfc347b3c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 1.2 在rviz里查看3D模型

**新建launch文件**

在robot_description/launch文件夹下新建display.launch文件，代码如下：

```
<?xml version="1.0"?>
<launch>
    <arg name="model" />
    <arg name="gui" default="False" />
    <param name="robot_description" textfile="$(arg model)" />
    <param name="use_gui" value="$(arg gui)"/>
    <node name="joint_state_publisher" pkg="joint_state_publisher" type="joint_state_publisher" />
    <node name="robot_state_publisher" pkg="robot_state_publisher" type="robot_state_publisher" />
    <node name="rviz" pkg="rviz" type="rviz" args="-d $(find urdf_tutorial)/urdf.rviz" />
</launch>
```
**运行launch文件**

```
roslaunch chapter4_tutorials display.launch model:=/home/cdq/dev/catkin_ws/src/chapter4_tutorials/robot1_description/robot1.urdf
```

注意：不要把冒号和等号写反了，文件位置信息可以把文件直接拖进终端就会显示出来。


显示如下：

![image](http://upload-images.jianshu.io/upload_images/16115686-aeb5a53348976628.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

默认状态下画面中什么都没有，此时需要做出调整。

在左下角的add按钮中添加RobotModel，然后将Fixed Frame选为base_link

![image](http://upload-images.jianshu.io/upload_images/16115686-f5dc96ecdd8fc799.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#  二、添加基座臂、连接臂和夹持臂

## 2.1 补充urdf文件

在</robot>前增添以下代码：
```html
<link name="arm_base">
<visual>
<geometry>
<box size="0.1 .1 .1"/>
</geometry>
<origin rpy="0 0 0" xyz="0 0 0.1"/>
<material name="white">
<color rgba="1 1 1 1"/>
</material>
</visual>
<collision>
<geometry>
<box size="0.1 .1 .1"/>
</geometry>
</collision>
<inertial>
<mass value="1"/>
<inertia ixx="1.0" ixy="0.0" ixz="0.0" iyy="1.0" iyz="0.0" izz="1.0"/>
</inertial>
</link>
<joint name="base_to_arm_base" type="continuous">
<parent link="base_link"/>
<child link="arm_base"/>
<axis xyz="0 0 1"/>
<origin xyz="0 0 0"/>
</joint>
<link name="arm_1">
<visual>
<geometry>
<box size="0.05 .05 0.5"/>
</geometry>
<origin rpy="0 0 0" xyz="0 0 0.25"/>
<material name="white">
<color rgba="1 1 1 1"/>
</material>
</visual>
<collision>
<geometry>
<box size="0.05 .05 0.5"/>
</geometry>
</collision>
<inertial>
<mass value="1"/>
<inertia ixx="1.0" ixy="0.0" ixz="0.0" iyy="1.0" iyz="0.0" izz="1.0"/>
</inertial>
</link>
<joint name="arm_1_to_arm_base" type="revolute">
<parent link="arm_base"/>
<child link="arm_1"/>
<axis xyz="1 0 0"/>
<origin xyz="0 0 0.15"/>
<limit effort ="1000.0" lower="-1.0" upper="1.0" velocity="0.5"/>
</joint>
<link name="arm_2">
<visual>
<geometry>
<box size="0.05 0.05 0.5"/>
</geometry>
<origin rpy="0 0 0" xyz="0.06 0 0.15"/>
<material name="white">
<color rgba="1 1 1 1"/>
</material>
</visual>
<collision>
<geometry>
<box size="0.05 .05 0.5"/>
</geometry>
</collision>
<inertial>
<mass value="1"/>
<inertia ixx="1.0" ixy="0.0" ixz="0.0" iyy="1.0" iyz="0.0" izz="1.0"/>
</inertial>
</link>
<joint name="arm_2_to_arm_1" type="revolute">
<parent link="arm_1"/>
<child link="arm_2"/>
<axis xyz="1 0 0"/>
<origin xyz="0.0 0 0.45"/>
<limit effort ="1000.0" lower="-2.5" upper="2.5" velocity="0.5"/>
</joint>
<joint name="left_gripper_joint" type="revolute">
<axis xyz="0 0 1"/>
<limit effort="1000.0" lower="0.0" upper="0.548" velocity="0.5"/>
<origin rpy="0 -1.57 0" xyz="0.06 0 0.4"/>
<parent link="arm_2"/>
<child link="left_gripper"/>
</joint>
<link name="left_gripper">
<visual>
<origin rpy="0 0 0" xyz="0 0 0"/>
<geometry>
<mesh filename="package://pr2_description/meshes/gripper_v0/l_finger.dae"/>
</geometry>
</visual>
<collision>
<geometry>
<box size="0.1 .1 .1"/>
</geometry>
</collision>
<inertial>
<mass value="1"/>
<inertia ixx="1.0" ixy="0.0" ixz="0.0" iyy="1.0" iyz="0.0" izz="1.0"/>
</inertial>
</link>
<joint name="left_tip_joint" type="fixed">
<parent link="left_gripper"/>
<child link="left_tip"/>
</joint>
<link name="left_tip">
<visual>
<origin rpy="0.0 0 0" xyz="0.09137 0.00495 0"/>
<geometry>
<mesh filename="package://pr2_description/meshes/gripper_v0/l_finger_tip.dae"/>
</geometry>
</visual>
<collision>
<geometry>
<box size="0.1 .1 .1"/>
</geometry>
</collision>
<inertial>
<mass value="1"/>
<inertia ixx="1.0" ixy="0.0" ixz="0.0" iyy="1.0" iyz="0.0" izz="1.0"/>
</inertial>
</link>
<joint name="right_gripper_joint" type="revolute">
<axis xyz="0 0 -1"/>
<limit effort="1000.0" lower="0.0" upper="0.548" velocity="0.5"/>
<origin rpy="0 -1.57 0" xyz="0.06 0 0.4"/>
<parent link="arm_2"/>
<child link="right_gripper"/>
</joint>
<link name="right_gripper">
<visual>
<origin rpy="-3.1415 0 0" xyz="0 0 0"/>
<geometry>
<mesh filename="package://pr2_description/meshes/gripper_v0/l_finger.dae"/>
</geometry>
</visual>
<collision>
<geometry>
<box size="0.1 .1 .1"/>
</geometry>
</collision>
<inertial>
<mass value="1"/>
<inertia ixx="1.0" ixy="0.0" ixz="0.0" iyy="1.0" iyz="0.0" izz="1.0"/>
</inertial>
</link>
<joint name="right_tip_joint" type="fixed">
<parent link="right_gripper"/>
<child link="right_tip"/>
</joint>
<link name="right_tip">
<visual>
<origin rpy="-3.1415 0 0" xyz="0.09137 0.00495 0"/>
<geometry>
<mesh filename="package://pr2_description/meshes/gripper_v0/l_finger_tip.dae"/>
</geometry>
</visual>
<collision>
<geometry>
<box size="0.1 .1 .1"/>
</geometry>
</collision>
<inertial>
<mass value="1"/>
<inertia ixx="1.0" ixy="0.0" ixz="0.0" iyy="1.0" iyz="0.0" izz="1.0"/>
</inertial>
</link>
```

## 2.2 显示3D图形

```
roslaunch chapter4_tutorials display.launch model:=/home/cdq/dev/catkin_ws/src/chapter4_tutorials/robot1_description/urdf/robot1.urdf　　
```


如果终端显示错误：

[rospack] Error: package 'pr2_description' not found........

 则需要安装pr2模型文件

```
sudo apt-get install ros-kinetic-pr2-common
```

如果显示如下警告，请忽视，程序正常运行：

libGL error: failed to create drawable
TIFFFieldWithTag: Internal error, unknown tag 0x829a.
TIFFFieldWithTag: Internal error, unknown tag 0x829d.
TIFFFieldWithTag: Internal error, unknown tag 0x8822.



## 2.3 使机器人运动 

配合joint_state_publisher，调用gui功能:

```
 roslaunch chapter4_tutorials display.launch model:=/home/cdq/dev/catkin_ws/src/chapter4_tutorials/robot1_description/urdf/robot1.urdf gui:=ture
```


显示如下:

![image](http://upload-images.jianshu.io/upload_images/16115686-b0d86061e200594f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

urdf文件中每一个axis对应一个调节器，joint_state_publisher应该是ros中自带的调节joint的功能，所以直接调用就可以。

如在arm_1_to_arm_base上所使用的限制通过<limit effort="1000.0"lower="-1.0"upper="1.0"velocity="0.5"/>这一行设置，可以用axis xyz="100"选择转动轴来运动。

<limit>标签用于选择以下属性：

effort（关节所承受的最大力），lower（赋值给关节的下限，旋转关节的单位是弧度，移动关节的单位是米），upper（赋值给关节的上限），velocity（强制关节的最大速度）。
