---
title: ROS-TF-Time
date: 2019-1-29
categories:
- 笔记
tags:
- ros
toc: true
img: /medias/paperimg/ros.jpg

---
如何在特定时间进行转换。让第二只乌龟去第一只乌龟在5秒前的地方。<!-- more -->
#  一、修改turtle_tf_listener.cpp文件

```
try{
    ros::Time now = ros::Time::now();
    ros::Time past = now - ros::Duration(5.0);
    listener.waitForTransform("/turtle2", now,
                              "/turtle1", past,
                              "/world", ros::Duration(1.0));//等待转换
    listener.lookupTransform("/turtle2", now,
                             "/turtle1", past,
                             "/world", transform);
     }
```

# 二、运行launch文件

```
roslaunch learning_tf start_demo.launch
```

![image](http://upload-images.jianshu.io/upload_images/16115686-181e1893b958910e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这样，第二只乌龟就会走到第一只乌龟5S前的位置。




参考自：[http://wiki.ros.org/tf/Tutorials/Time%20travel%20with%20tf%20%28C%2B%2B%29](http://wiki.ros.org/tf/Tutorials/Time%20travel%20with%20tf%20%28C%2B%2B%29)
