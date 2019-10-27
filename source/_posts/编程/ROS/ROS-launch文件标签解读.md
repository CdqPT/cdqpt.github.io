---
title: ROS-launch文件标签解读
date: 2019-1-27
categories:
- 笔记
tags:
- ros
toc: true
img: /medias/paperimg/ros.jpg

---
ROS提供了一个同时启动节点管理器（master）和多个节点的途径，即使用启动文件（launch file）。事实上，在ROS功能包中，启动文件的使用是非常普遍的。任何包含两个或两个以上节点的系统都可以利用启动文件来指定和配置需要使用的节点。通常的命名方案是以.launch作为启动文件的后缀，启动文件是XML文件。一般把启动文件存储在取名为launch的目录中。

启动launch文件可以会自动启动roscore，相对方便一些。<!-- more -->
# 一、launch文件用法
```
roslaunch package-name launch-file-name
```
注意:通过apt-get安装的软件包可以直接运行命令，但自己编译的软件包必须在launch文件夹目录下运行。

# 二、launch文件基本标签
```
<launch>
    <node pkg="turtlesim" type="turtlesim_node" name="sim"/>
    <node pkg="turtlesim" type="turtle_teleop_key" name="teleop" output="screen"/>
</launch>
```


其中：

<launch>是launch文件的文件标签，是一定要有的成分
<node>是节点标签
pkg是要启动节点所在的功能包名，type是节点的可执行文件名，就是修改cmakelist文件时的那个名称，这两个属性等同于在终端中使用rosrun命令执行节点时的输入参数，

name是用来定义节点运行时的名称，将覆盖节点中init()赋予节点的名称，可以理解为重命名。有的解析说是cpp文件中的名称，然而我测试过，改成什么都行。name的作用是

为了重复利用节点，比如发布广播t1和发布广播t2都使用发布广播节点，但运行时名称不同。

<node pkg="learning_tf" type="turtle_tf_broadcaster" args="/turtle1" name="turtle1_tf_broadcaster" />
<node pkg="learning_tf" type="turtle_tf_broadcaster" args="/turtle2" name="turtle2_tf_broadcaster" />
其他：

·  output = "screen"：将节点的标准输出打印到终端屏幕，默认输出为日志文档；

·  respawn = "true"：复位属性，该节点停止时，会自动重启，默认为false；

·  required = "true"：必要节点，当该节点终止时，launch文件中的其他节点也被终止；

·  ns = "namespace"：命名空间，为节点内的相对名称添加命名空间前缀；

·  args = "arguments"：节点需要的输入参数。

# 三、launch文件其他常用标签
## 3.1 <param>标签
launch文件允许设置和修改参数，就是通过param标签设置。这个功能类似于修改变量。

<param name="output_frame" value="odom"/>
其中，

name是参数名，value是参数值。

也可以加载参数文件：

<rosparamfile="$(find 2dnav_pr2)/config/costmap_common_params.yaml" command="load" ns="local_costmap" />
command可以是加载或卸载，ns是命名空间。

 ## 3.2  <arg>
 只针对launch文件生效，不对cpp文件生效。