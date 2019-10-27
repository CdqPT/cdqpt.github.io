---
title: ROS-TF-监听
date: 2019-1-27
categories:
- 笔记
tags:
- ros
toc: true
img: /medias/paperimg/ros.jpg

---
监听第一只海龟的位置，然后让第二只海龟跟随第一只海龟。<!-- more -->
通过监听tf，我们可以避免繁琐的旋转矩阵的计算，而直接获取我们需要的相关信息。 

# 一、新建cpp文件

新建turtle_tf_listener.cpp文件，内容如下：

```
#include <ros/ros.h>
#include <tf/transform_listener.h>//监听类TransformListener 的头文件，该对象会自动订阅ROS中的tf消息，并且管理所有的变换关系数据。
#include <geometry_msgs/Twist.h>
#include <turtlesim/Spawn.h>

int main(int argc, char** argv){
  ros::init(argc, argv, "my_tf_listener");

  ros::NodeHandle node;
//生成一只新乌龟
  ros::service::waitForService("spawn");
  ros::ServiceClient add_turtle =
    node.serviceClient<turtlesim::Spawn>("spawn");
  turtlesim::Spawn srv;
  add_turtle.call(srv);

  ros::Publisher turtle_vel =
    node.advertise<geometry_msgs::Twist>("turtle2/cmd_vel", 10);//定义发布者turtle_vel

  tf::TransformListener listener;//新建监听对象

  ros::Rate rate(10.0);
  while (node.ok()){
    tf::StampedTransform transform;//定义存放转换信息（平动，转动）的变量

    try{
      listener.lookupTransform("/turtle2", "/turtle1",
                               ros::Time(0), transform);//可以获得两个坐标系之间转换的关系，包括旋转与平移。转换得出的坐标是在“/turtle2”坐标系下的 
    }
    catch (tf::TransformException &ex) {
      ROS_ERROR("%s",ex.what());
      ros::Duration(1.0).sleep();
      continue;
    }//由于tf的会把监听的内容存放到一个缓存中，然后再读取相关的内容，而这个过程可能会有几毫秒的延迟，也就是，tf的监听器并不能监听到“现在”的变换，所以如果不使用try,catch函数会卡死。
//这个转换基于turtle1的距离和角度，用来计算turtle2的新的线速度和角速度，新的速度被发布在"turtle2/cmd_vel" 话题上，并且sim将使用它来更新turtle2的移动
    geometry_msgs::Twist vel_msg;
    vel_msg.angular.z = 4.0 * atan2(transform.getOrigin().y(),
                                    transform.getOrigin().x());
    vel_msg.linear.x = 0.5 * sqrt(pow(transform.getOrigin().x(), 2) +
                                  pow(transform.getOrigin().y(), 2));
    turtle_vel.publish(vel_msg);//发布话题，控制turtle2的位置。

    rate.sleep();
  }
  return 0;
};
```

总体思路是：新建监听对象listener，并将turtle1的位置信息转化成vel_msg信息，然后使用发布者turtle_vel发布出去。

# 二、修改launch文件

在launch文件末尾添加

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
    <node pkg="learning_tf" type="turtle_tf_broadcaster"
          args="/turtle2" name="turtle2_tf_broadcaster" />
    <node pkg="learning_tf" type="turtle_tf_listener"
          name="listener" />
  </launch>
```

# 三、运行

```
roslaunch learning_tf start_demo.launch
```
![image](http://upload-images.jianshu.io/upload_images/16115686-475967a740daaec7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 参考自：[http://wiki.ros.org/tf/Tutorials/Writing%20a%20tf%20listener%20%28C%2B%2B%29](http://wiki.ros.org/tf/Tutorials/Writing%20a%20tf%20listener%20%28C%2B%2B%29)
