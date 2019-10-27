---
title: ROS-turtlesim功能包演示
date: 2018-12-28
categories:
- 笔记
tags:
- ros
toc: true
img: /medias/paperimg/ros.jpg

---
turtlesim是ros自带的一个功能包，应该是用于基础教学的功能包，帮助新手入门的一个实例，包括：节点，主题，服务以及参数的应用。通过学习使用turtlesim功能包可以了解ros的一些基础命令与通信架构。<!-- more -->
# 一、启动ROS Master

```
roscore
``` 

# 二、启动小海龟仿真器节点

```
 rosrun turtlesim turtlesim_node  
```

# 三、启动小海龟键盘控制节点

```
 rosrun turtlesim turtle_teleop_key
```

说明：
turtlesim     - 功能包名称

turtlesim_node    - 节点的名称，是在功能包turtlesim 下面

# 四、启动计算图节点

```
rqt_graph
```

# 五、查看当前系统启动的所有节点

```
rosnode list
```

显示如下：
/rosout
/rqt_gui_py_node_7316
/teleop_turtle
/turtlesim

# 六、查看具体节点的信息

```
rosnode info /turtlesim
```
显示如下：
Node [/turtlesim]            节点的名字
Publications:     该节点所发布的话题
 * /rosout [rosgraph_msgs/Log]
 * /turtle1/color_sensor [turtlesim/Color]
 * /turtle1/pose [turtlesim/Pose]

Subscriptions:  该节点订阅的话题
 * /turtle1/cmd_vel [geometry_msgs/Twist]

Services:     该节点使用的服务
 * /clear
 * /kill
 * /reset
 * /spawn
 * /turtle1/set_pen
 * /turtle1/teleport_absolute
 * /turtle1/teleport_relative
 * /turtlesim/get_loggers
 * /turtlesim/set_logger_level

contacting node http://cdq1:33854/ ...
Pid: 21222    节点的ID号
Connections:    
 * topic: /rosout
    * to: /rosout
    * direction: outbound
    * transport: TCPROS  通信协议
 * topic: /turtle1/cmd_vel
    * to: /teleop_turtle (http://vm:43869/)
    * direction: inbound

    * transport: TCPROS

# 七、查看系统中发布和订阅的话题

```
rostopic list
```

显示如下：
/rosout
/rosout_agg
/statistics
/turtle1/cmd_vel
/turtle1/color_sensor
/turtle1/pose

# 八、查看话题的信息 

```
rostopic info /turtle1/cmd_vel 
```

显示如下：
Type: geometry_msgs/Twist      消息类型

Publishers:     发布者是 /teleop_turtle
 * /teleop_turtle (http://cdq1:408083/)

Subscribers:  订阅者是  /turtlesim 节点

 * /turtlesim (http://cdq1:33853/)

# 九、监听话题信息

```
rostopic echo /turtle1/cmd_vel ；
```

在rosrun turtlesim turtle_teleop_key终端下控制方向键.


显示如下：

linear:     线速度的单位是 m/s
  x: 2.0
  y: 0.0
  z: 0.0
angular:  角速度的单位是 rad/s
  x: 0.0
  y: 0.0
  z: 0.0

# 十、发布话题
  //画弧

```
rostopic pub -1 /turtle1/cmd_vel geometry_msgs/Twist -- '[2.0, 0.0, 0.0]' '[0.0, 0.0, 1.8]'      
```

 // 画圈

```
rostopic pub /turtle1/cmd_vel geometry_msgs/Twist -r 1 -- '[2.0, 0.0, 0.0]' '[0.0, 0.0, 1.8]'   
``` 

其中，

-1 这个参数的意思是只发布一个命令，然后退出；

/turtle1/cmd_vel    要发布消息的话题；

geometry_msgs/Twist     要发布的消息类型名称；

--     （双破折号）这会告诉命令选项解析器接下来的参数部分都不是命令选项。这在参数里面包含有破折号-（比如负号）时是必须要添加的；

‘[2.0, 0.0, 0.0]’ ‘[0.0, 0.0, 1.8]’       在x轴坐标上以每秒2.0 m的速度移动,以z轴为中心,每秒旋转1.8rad；2.0是linear的值，1.8是angular的值；

rostopic pub用法：rostopic pub [topic] [msg_type] [args]；

-r 1  -r发布一个稳定的命令流，1，频率为1HZ。

# 十一、查看系统中的服务

```
rosservice list
```

显示如下：

/clear
/kill
/reset
/rosout/get_loggers
/rosout/set_logger_level
/rostopic_7788_1524147136200/get_loggers
/rostopic_7788_1524147136200/set_logger_level
/rqt_gui_py_node_6138/get_loggers
/rqt_gui_py_node_6138/set_logger_level
/spawn
/teleop_turtle/get_loggers
/teleop_turtle/set_logger_level
/turtle1/set_pen
/turtle1/teleport_absolute
/turtle1/teleport_relative
/turtlesim/get_loggers

/turtlesim/set_logger_level

# 十二、查看服务的信息

```
rosservice info /spawn 
```

显示如下：

Node: /turtlesim
URI: rosrpc://vm:42961
Type: turtlesim/Spawn

Args: x y theta name

# 十三、新增一只海龟

```
rosservice call spawn 2 2 0 “turtle2”
```

在(2,2)坐标点，0度方向，新生一只名字为turtle2的乌龟。

# 十四、查看修改参数

查询运行中的参数：
```
rosparam list
```

显示如下：

/background_b
/background_g
/background_r
/rosdistro
/roslaunch/uris/host_cdq1__38893
/rosversion
/run_id

获取background_b背景参数 :
```
rosparam get background_b 
```
修改background_b背景参数 :
```
rosparam get background_b 100
```
此时背景颜色并没有改变，需要刷新一下：
```
rosservice call clear
```