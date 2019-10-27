
---
title: ROS-节点-Topic
date: 2018-12-30
categories:
- 笔记
tags:
- ros
toc: true
img: /medias/paperimg/ros.jpg

---


本部分主要介绍ros一些基础功能的使用，包括创建和编译工作空间、功能包、节点以及话题。<!-- more -->

# 第一种方式：使用roboware studio软件操作

## 1.1 创建工作空间



![图111](https://upload-images.jianshu.io/upload_images/16115686-1a8ac11c1aa9bc9d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



回车然后点击保存。

## 1.2 新建功能包

功能包名为：chapter2_tutorials std_msgs roscpp

注：chapter2_tutorials是功能包名，std_msgs 和roscpp是依赖。

![image](http://upload-images.jianshu.io/upload_images/16115686-2760ba29f1fd68e5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 1.3 新建源文件

在chapter2_tutorials包下的src中新建两个.cpp文件，分别命名为example1_a.cpp和example1_b.cpp。

注意新建cpp文件时选择第二项“加入到新的可执行文件中”，这样roboware就会自动修改cmake文件了。

![image](http://upload-images.jianshu.io/upload_images/16115686-874f4714c75e683e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

example1_a.cpp的内容： 

```c
#include "ros/ros.h"   //包含了使用ros节点所有必要的文件
#include "std_msgs/String.h"  //包含了要使用的消息类型
#include <sstream>

 int main(int argc, char *argv[])
{
    ros::init(argc, argv, "example1_a");   //初始化节点并设置其名称，该名称是唯一的
    ros::NodeHandle n;   //进程处理程序
    ros::Publisher chatter_pub= n.advertise<std_msgs::String>("message",1000);  //将该节点设置为发布者，话题为message，消息类型为std_msgs::String，缓存1000条信息
    ros::Rate loop_rate(10);      //发送频率为10HZ
    while (ros::ok())               //判断是否有节点运行
    {
        std_msgs::String msg;       //创建消息变量msg  
        std::stringstream ss;       //创建一个字符串变量ss
        ss<< "I am the example1_a node";  //字符串ss的内容
        msg.data=ss.str();           //给msg赋值
        chatter_pub.publish(msg);    //发布者发送消息
        ros::spinOnce();             //运行主循环
        loop_rate.sleep();           //按照10HZDE频率将程序挂起
    }
    return 0;
}
```

example1_b.cpp的内容：

```c
#include "ros/ros.h"
#include "std_msgs/String.h"

void chatterCallback(const std_msgs::String::ConstPtr& msg)   //回调函数，当收到消息时调用此函数
{
  ROS_INFO("I [%s]",msg->data.c_str());   //输出信息,[%s]这里是小写s
}

 int main(int argc, char *argv[])
{
    /* code for main function */
    ros::init(argc, argv, "example1_b");      
    ros::NodeHandle n;
    ros::Subscriber sub =n.subscribe("message",1000,chatterCallback);    //创建一个订阅者，处理信息的回调函数为chatterCallback
    ros::spin();
    return 0;
}
```


## 1.4 编译

![image](http://upload-images.jianshu.io/upload_images/16115686-8ea7c0f4c558bd64.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 1.5 添加环境变量

ctrl+` 呼出控制台；


这一步很重要，很多时候程序运行不了，就是因为没有添加环境变量.

```
echo "source ~/catkin_ws/devel/setup.bash" >> ~/.bashrc
cd ~/catkin_ws/
source devel/setup.bash
```

注：~/catkin_ws修改为自己工作空间的位置。这句话也可以在home目录下ctrl+h显示隐藏文件，打开.bashrc，在最后一行找到或添加。

## 1.6 运行

启动节点管理器

```
roscore
```

运行节点

```
rosrun chapter2_tutorials example1_a
rosrun chapter2_tutorials example1_b
```

-----------------------------------------------

输出如下：

![image](http://upload-images.jianshu.io/upload_images/16115686-11e4e99377495765.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 第二种方式：使用命令行编写

## 2.1 创建和编译工作空间

### 2.1.1 查看正在使用的工作空间:

```
echo $ROS_PACKAGE_PATH
```

-----------------------------------------------

显示如下：

/opt/ros/kinetic/share

### 2.1.2 创建工作空间文件夹：

```
mkdir -p ~/dev/catkin_ws/src
```

-----------------------------------------------

其中：

mkdir是新建文件夹命令，-p是如果没有路径上的文件夹则新建文件夹，～是根目录。

### 2.1.3 初始化工作空间：

```
cd ~/dev/catkin_ws/src
catkin_init_workspace
```

-----------------------------------------------

显示如下：

Creating symlink "/home/cdq/dev/catkin_ws/src/CMakeLists.txt" pointing to "/opt/ros/kinetic/share/catkin/cmake/toplevel.cmake"

### 2.1.4 编译工作空间

```
cd ~/dev/catkin_ws
catkin_make
```

-----------------------------------------------

最后显示如下：

```
####
#### Running command: "make -j4 -l4" in "/home/cdq/dev/catkin_ws/build"
####
```

### 2.1.5 加载setup.bash文件

```
source devel/setup.bash
```

## 2.2 创建和编译功能包

### 2.2.1 创建新功能包

```
cd ~/dev/catkin_ws/src
catkin_create_pkg chapter2_tutorials std_msgs roscpp
```

-----------------------------------------------

显示如下：

Created file chapter2_tutorials/CMakeLists.txt
Created file chapter2_tutorials/package.xml
Created folder chapter2_tutorials/include/chapter2_tutorials
Created folder chapter2_tutorials/src
Successfully created files in /home/cdq/dev/catkin_ws/src/chapter2_tutorials. Please adjust the values in package.xml.

-----------------------------------------------

其中：

catkin_create_pkg 是创建新功能包命令，chapter2_tutorials是功能包名称，std_msgs和roscpp是依赖项。

------------------------------------

### 2.2.2 编译功能包

```
cd ~/dev/catkin_ws/
catkin_make
```

-----------------------------------------------


最后显示如下：
```

####
#### Running command: "make -j4 -l4" in "/home/cdq/dev/catkin_ws/build"
####
```
## 2.3 创建和编译节点

### 2.3.1 切换路径

```
roscd chapter2_tutorials/src
```
-----------------------------------------------

辨析：roscd和cd

roscd  直接加包名 例：roscd roscpp；

cd 加完整路径 例：cd /opt/ros/kinetic/share/roscpp

### 2.3.2 创建两个.cpp文件

在上一步的路径上建立（我是手动建立的）两个文件分别命名为example1_a.cpp和example1_b.cpp；

example1_a.cpp的内容： 

```c
#include "ros/ros.h"   //包含了使用ros节点所有必要的文件
#include "std_msgs/String.h"  //包含了要使用的消息类型
#include <sstream>

 int main(int argc, char *argv[])
{
    ros::init(argc, argv, "example1_a");   //初始化节点并设置其名称，该名称是唯一的
    ros::NodeHandle n;   //进程处理程序
    ros::Publisher chatter_pub= n.advertise<std_msgs::String>("message",1000);  //将该节点设置为发布者，话题为message，消息类型为std_msgs::String，缓存1000条信息
    ros::Rate loop_rate(10);      //发送频率为10HZ
    while (ros::ok())               //判断是否有节点运行
    {
        std_msgs::String msg;       //创建消息变量msg  
        std::stringstream ss;       //创建一个字符串变量ss
        ss<< "I am the example1_a node";  //字符串ss的内容
        msg.data=ss.str();           //给msg赋值
        chatter_pub.publish(msg);    //发布者发送消息
        ros::spinOnce();             //运行主循环
        loop_rate.sleep();           //按照10HZDE频率将程序挂起
    }
    return 0;
}
```

example1_b.cpp的内容：

```c
#include "ros/ros.h"
#include "std_msgs/String.h"

void chatterCallback(const std_msgs::String::ConstPtr& msg)   //回调函数，当收到消息时调用此函数
{
  ROS_INFO("I [%s]",msg->data.c_str());   //输出信息,[%s]这里是小写s
}

 int main(int argc, char *argv[])
{
    /* code for main function */
    ros::init(argc, argv, "example1_b");      
    ros::NodeHandle n;
    ros::Subscriber sub =n.subscribe("message",1000,chatterCallback);    //创建一个订阅者，处理信息的回调函数为chatterCallback
    ros::spin();
    return 0;
}
```

### 2.3.3 修改CMakeLists.txt文件

打开chapter2_tutorials包下的CMakeLists.txt文件，将以下命令行复制到文件的末尾处：

```
add_executable(example1_a1 src/example1_a.cpp) #将example1_a.cpp文件编译成可执行文件example1_a1 
add_executable(example1_b1 src/example1_b.cpp)

target_link_libraries(example1_a1 ${catkin_LIBRARIES}) #为可执行文件example1_a1添加链接库
target_link_libraries(example1_b1 ${catkin_LIBRARIES})
```

 注：此处有坑，可执行文件的名字（example1_a1 ）不能和节点原文件名字（example1_a.cpp）相同，不知道为什么。但节点原文件名字和文件内部的节点名字可以相同。

### 2.3.4 编译文件

 使用catkin_make工具来编译包和全部的节点：

```
cd ~/dev/vstkin_ws/
catkin_make --pkg chapter2_tutorials
```

## 2.4 运行节点

### 2.4.1 启动节点管理器

```
roscore
```

### 2.4.2 运行节点

```
rosrun chapter2_tutorials example1_a1
rosrun chapter2_tutorials example1_b1
```

注：若节点无法运行，每次开启节点前先在 ~/dev/catkin_ws/ 目录下执行命令行：source devel/setup.bash,或者一次性解决 ，输入指令  echo "source ~/dev/catkin_ws/devel/setup.bash" >> ~/.bashrc，红色部分为自己工作空间的名字。

-----------------------------------------------

输出如下：

![image](http://upload-images.jianshu.io/upload_images/16115686-cd0a2e06e6e8c85c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

