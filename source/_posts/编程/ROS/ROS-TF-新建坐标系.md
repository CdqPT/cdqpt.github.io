---
title: ROS-TF-新建坐标系
date: 2019-1-29
categories:
- 笔记
tags:
- ros
toc: true
img: /medias/paperimg/ros.jpg

---
在前面的试验中，我们分别有wolrd，turtle1和turtle2三个坐标系，并且world是turtle1和turtle2的父坐标系。现在我们来新建一个自定义坐标系，让turtle2跟着新的坐标系”carrot“运动。<!-- more -->
![image.png](https://upload-images.jianshu.io/upload_images/16115686-5854ae82e8d5b053.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
 # 一、新建源文件
 新建frame_tf_broadcaster.cpp文件，内容如下：
```C++
#include <ros/ros.h>
#include <tf/transform_broadcaster.h>

int main(int argc, char** argv){
  ros::init(argc, argv, "my_tf_broadcaster");
  ros::NodeHandle node;

  tf::TransformBroadcaster br;
  tf::Transform transform;

  ros::Rate rate(10.0);
  while (node.ok()){

    transform.setOrigin( tf::Vector3(0.0, 2.0, 0.0) );//设置新坐标系相对位置关系
    transform.setRotation( tf::Quaternion(0, 0, 0, 1) );//设置新坐标系相对旋转关系
    br.sendTransform(tf::StampedTransform(transform, ros::Time::now(), "turtle1", "carrot1"));//创建一个新坐标系carrot1，距离父坐标系turtle1距离2米
    rate.sleep();
  }
  return 0;
};
```
# 二、修改launch文件
在launch文件中添加代码：

```
<launch>
    ...
    <node pkg="learning_tf" type="frame_tf_broadcaster"
          name="broadcaster_frame" />
  </launch>
```

# 三、修改广播信息

修改turtle_tf_listener.cpp文件

```
listener.lookupTransform("/turtle2", "/carrot1", ros::Time(0), transform);
```
# 四、运行

编译并运行launch文件

```
roslaunch learning_tf start_demo.launch
```

 ![image](http://upload-images.jianshu.io/upload_images/16115686-363df6e00039901f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

现在turtle2始终跟随在carrot坐标系运动。


参考自：[http://wiki.ros.org/tf/Tutorials/Adding%20a%20frame%20%28C%2B%2B%29](http://wiki.ros.org/tf/Tutorials/Adding%20a%20frame%20%28C%2B%2B%29)
