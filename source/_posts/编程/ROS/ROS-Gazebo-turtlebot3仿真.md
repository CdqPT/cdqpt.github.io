---
title: ROS-Gazebo-turtlebot3仿真
date: 2019-1-9
categories:
- 笔记
tags:
- ros
img: /medias/paperimg/ros.jpg
toc: true
---
Gazebo是一款强大的3D仿真器,支持机器人开发所需的机器人、传感器和环境模型,并且通过搭载的物理引擎可以得到逼真的仿真结果。即便Gazebo是一款开源仿真器,却具有高水准的仿真性能,因此在机器人工程领域中非常流行。<!-- more -->
前提：已安装了turtlebot3软件包，如没有安装，可参考：[https://www.cnblogs.com/chendeqiang/p/10227401.html](https://www.cnblogs.com/chendeqiang/p/10227401.html)


# 一、 启动“世界”仿真图像

```
roslaunch turtlebot_gazebo turtlebot_world.launch 
```

显示如下：

![image](http://upload-images.jianshu.io/upload_images/16115686-67b12605f38f0d0c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

shift+鼠标左键 可以调整视角。

注：如果启动失败，可以注销后再重新打开就可以正常显示了。

# 二、启动键盘控制

启动键盘控制：

```
roslaunch turtlebot_teleop keyboard_teleop.launch
```

# 三、启动rviz查看turtlbot摄像机采集的信息

```
roslaunch turtlebot_rviz_launchers view_robot.launch 
```

勾选左边的depthcloud就可以看到，记得切换一下topic。按鼠标左键可调整视角。

 -----------------------------------------

显示如下：

![image](http://upload-images.jianshu.io/upload_images/16115686-cec4dec3b5672f13.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 四、最终版

左边是gazebo，右边是rviz，前端是键盘控制。

![image](http://upload-images.jianshu.io/upload_images/16115686-b38c1c60a6c3ea7c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#  五、launch文件解析

```
<launch>

    <!-- 设置launch文件的参数 -->
    <arg name="world_name" value="$(find mrobot_gazebo)/worlds/playground.world"/>
    <arg name="paused" default="false"/>
    <arg name="use_sim_time" default="true"/>
    <arg name="gui" default="true"/>
    <arg name="headless" default="false"/>
    <arg name="debug" default="false"/>

    <!-- 运行gazebo仿真环境 -->
    <include file="$(find gazebo_ros)/launch/empty_world.launch">
        <arg name="world_name" value="$(arg world_name)" />
        <arg name="debug" value="$(arg debug)" />
        <arg name="gui" value="$(arg gui)" />
        <arg name="paused" value="$(arg paused)"/>
        <arg name="use_sim_time" value="$(arg use_sim_time)"/>
        <arg name="headless" value="$(arg headless)"/>
    </include>

    <!-- 加载机器人模型描述参数 -->
    <param name="robot_description" command="$(find xacro)/xacro --inorder '$(find mrobot_gazebo)/urdf/mrobot.urdf.xacro'" /> 

    <!-- 运行joint_state_publisher节点，发布机器人的关节状态  -->
    <node name="joint_state_publisher" pkg="joint_state_publisher" type="joint_state_publisher" ></node> 

    <!-- 运行robot_state_publisher节点，发布tf  -->
    <node name="robot_state_publisher" pkg="robot_state_publisher" type="robot_state_publisher"  output="screen" >
        <param name="publish_frequency" type="double" value="50.0" />
    </node>

    <!-- 在gazebo中加载机器人模型-->
    <node name="urdf_spawner" pkg="gazebo_ros" type="spawn_model" respawn="false" output="screen"
          args="-urdf -model mrobot -param robot_description"/> 

</launch>
```

launch文件主要做了两件事:

1.启动机器人的状态发布节点，同时加载带有Gazebo属性的机器人urdf模型；

2.启动Gazebo，并且将机器人模型加载到Gazebo仿真环境中。

# 六、运行摄像头仿真

启动仿真环境

```
roslaunch view_mrobot_with_camera_gazebo.launch
```
启动rqt

```
rqt_image_view
```