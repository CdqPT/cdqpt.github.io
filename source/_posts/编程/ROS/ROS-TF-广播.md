---
title: ROS-TF-广播
date: 2019-1-27
categories:
- 笔记
tags:
- ros
toc: true
img: /medias/paperimg/ros.jpg

---
将海龟的坐标系变换广播到TF。<!-- more -->
URDF文件的描述是在相对坐标上进行的，运动起来就需要考虑机器人各个连杆的相对位置关系。TF的诞生就是为了自动管理这些相对关系下的坐标变换的，而我们需要做的就是给出连杆之间的相对位置关系，剩下的在运动中的坐标变换，速度关系，位置等信息就都由TF功能包自动处理完成。更直观的描述请参考：[http://www.guyuehome.com/355](http://www.guyuehome.com/355)

坐标标签有：位置坐标x，y，z和旋转角roll,pitch,yaw。例如如果只在平面移动，则只需要x,y,yaw即可。

# 一、新建功能包

在工作空间中新建功能包learning_tf tf roscpp rospy turtlesim

# 二、新建cpp文件

新建turtle_tf_broadcaster.cpp文件，文件内容如下：

```C++
#include <ros/ros.h>
#include <tf/transform_broadcaster.h>
#include <turtlesim/Pose.h>

std::string turtle_name;

void poseCallback(const turtlesim::PoseConstPtr& msg){
  static tf::TransformBroadcaster br;
  tf::Transform transform;//定义存放转换信息（平动，转动）的变量
  transform.setOrigin( tf::Vector3(msg->x, msg->y, 0.0) );//设置坐标原点
//定义旋转
  tf::Quaternion q;
  q.setRPY(0, 0, msg->theta);
  transform.setRotation(q);

  br.sendTransform(tf::StampedTransform(transform, ros::Time::now(), "world", turtle_name));//新建坐标系turtle_name，父坐标系为world，坐标系相对位置为transform
}

int main(int argc, char** argv){
  ros::init(argc, argv, "my_tf_broadcaster");
  if (argc != 2){ROS_ERROR("need turtle name as argument"); return -1;};
  turtle_name = argv[1];

  ros::NodeHandle node;
  ros::Subscriber sub = node.subscribe(turtle_name+"/pose", 10, &poseCallback);

  ros::spin();
  return 0;
};
```

**代码解释**

大概思路是：接收turtle1的位置话题（/turtle1/pose），此时调用回调函数，将位置信息发布到tf

include <tf/transform_broadcaster.h>，TF功能包提供了一个广播工具TransformBroadcaster，这个类需要引用<tf/transform_broadcaster.h>头文件

```
  tf::Transform transform;
  transform.setOrigin( tf::Vector3(msg->x, msg->y, 0.0) );
  tf::Quaternion q;
  q.setRPY(0, 0, msg->theta);
```

这里创建了一个变换实例，并将2D位姿信息转化为3D位姿信息。

```
transform.setRotation(q);
```
这里设置了旋转

```
br.sendTransform(tf::StampedTransform(transform, ros::Time::now(), "world", turtle_name));
```
使用TransformBroadcaster发送TF变换需要四个参数：

1.  变换本身
2.  时间戳
3.  提供连杆的父框架，这里是world
4.  提供连杆的子框架，即自身，这里是乌龟本身。

# 三、新建launch文件

新建start_demo.launch文件，文件内容如下：

```
<launch>
    <!-- Turtlesim Node-->
    <node pkg="turtlesim" type="turtlesim_node" name="sim"/>

    <node pkg="turtlesim" type="turtle_teleop_key" name="teleop" output="screen"/>
    <!-- Axes -->
    <param name="scale_linear" value="2" type="double"/>
    <param name="scale_angular" value="2" type="double"/>

    <node pkg="learning_tf" type="turtle_tf_broadcaster"
          args="/turtle1" name="turtle1_tf_broadcaster" />
  </launch>
```

运行launch文件

```
roslaunch learning_tf start_demo.launch
```

# 四、查看广播

```
rosrun tf tf_echo /world /turtle1
```
![image](http://upload-images.jianshu.io/upload_images/16115686-71d14702218a5495.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 参考自：[http://wiki.ros.org/tf/Tutorials/Writing%20a%20tf%20broadcaster%20%28C%2B%2B%29](http://wiki.ros.org/tf/Tutorials/Writing%20a%20tf%20broadcaster%20%28C%2B%2B%29)
