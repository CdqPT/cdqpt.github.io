---
title: ROS-service
date: 2018-12-31
categories:
- 笔记
tags:
- ros
toc: true
img: /medias/paperimg/ros.jpg

---
理解Message，Topic和Service。<!-- more -->
**消息（Message）：**消息就是要发送的信息，话题和服务都要发布消息。

**话题（Topic）：**话题相当于QQ群的群名称，作用是给所有订阅该话题的节点提供一个“聊天平台”。

**服务（Service）：**服务相当于私聊，作用是给两个需要直接私聊的成员提供“聊天平台”。

注：以下操作都是在roboware下进行的，如果使用命令行操作请参考官网例程：[http://wiki.ros.org/cn/ROS/Tutorials/CreatingMsgAndSrv](http://wiki.ros.org/cn/ROS/Tutorials/CreatingMsgAndSrv)

# 一、创建msg和service

## 1.1 创建msg文件夹

在chapter2_tutorials包下创建msg文件夹，并在msg文件夹下新建chapter2_msg1.msg文件，添加如下内容：

```
int32 A
int32 B
int32 C
```

## 1.2 编译工程及检验（可选操作）

编译：点击锤子编译工程。

检验：在终端输入

```
rosmsg show chapter2_tutorials/chapter2_msg1
```

显示如下：

int32 A
int32 B
int32 C

说明编译正确。

## 1.3 创建srv文件

在chapter2_tutorials文件夹下创建一个名为srv的文件夹，并在srv文件夹下新建chapter2_srv1.srv文件，添加内容如下：

```
int32 A
int32 B
int32 C
---
int32 sum
```

注：不知道 --- 是什么

## 1.4 编译工程（可选操作）

编译：点击锤子编译工程。

检验：在终端输入

```
rossrv show chapter2_tutorials/chapter2_srv1
```

显示如下：

int32 A
int32 B
int32 C
**---**
int32 sum

说明编译正确。

## 1.5 新建节点

在chapter2_tutorials包下新建两个节点example2_a.cpp和example2_b.cpp，添加代码如下：

example2_a.cpp

```c++
#include "ros/ros.h"
#include "chapter2_tutorials/chapter2_srv1.h"  //是由之前我们创建的srv文件自动产生的头文件。

bool add(chapter2_tutorials::chapter2_srv1::Request  &req,               
         chapter2_tutorials::chapter2_srv1::Response &res)//这个函数提供两个int值求和的服务，int值从request里面获取，而返回数据装入response内，这些数据类型都定义在srv文件内部，函数返回一个boolean值。
 {
   res.sum=req.A+req.B+req.C;                  //求3个变量的和
   ROS_INFO("request:A=%1d,B=%1d C=%1d",(int)req.A,(int)req.B,(int)req.C);
   ROS_INFO("sending back response:[%1d]",(int)res.sum);
   return true;
 }


 int main(int argc, char *argv[])
{
     ros::init(argc, argv, "add_3_ints_server");//初始化节点
     ros::NodeHandle n;  
     ros::ServiceServer service=n.advertiseService("add_3_ints",add);  //创建名为service的服务并在ROS中发布广播，add_3_ints是，add是回调函数
     ROS_INFO("Ready to add 3 ints.");
     ros::spin();

     return 0;
 }
```

example2_b.cpp

```c++
#include "ros/ros.h"
#include"chapter2_tutorials/chapter2_srv1.h"
#include <cstdlib>

int main(int argc, char *argv[])
{
    ros::init(argc, argv, "add_3_ints_client");
    if (argc!=4)
    {
        /* code for True */
        ROS_INFO("usage: add_3_ints_client A B C");
        return 1;
    }
    ros::NodeHandle n;
    ros::ServiceClient client=n.serviceClient<chapter2_tutorials::chapter2_srv1>("add_3_ints");//创建一个名为client的客户端，chapter2_srv1是服务名称，
    chapter2_tutorials::chapter2_srv1 srv; //创建srv请求类型的一个实例
    srv.request.A=atoll(argv[1]);
    srv.request.B=atoll(argv[2]);
    srv.request.C=atoll(argv[3]);
    if (client.call(srv))   //如果调用成功
    {
        /* code for True */
        ROS_INFO("SUM:%1d",(long int)srv.response.sum);  
    }
    else
    {
        ROS_ERROR("Failed to call service add_3_ints");
        return 1;
    }
    
    return 0;
}
```

## 1.6 编译



## 1.7 运行

 ```
roscore

rosrun chapter2_tutorials example2_a1
```

显示如下：

[ INFO] [1546309142.653888277]: Ready to add 3 ints.

在新终端输入指令：

```
rosrun chapter2_tutorials example2_b1 1 2 3
```


终端2显示如下：

[ INFO] [1546309198.935606535]: SUM:6

终端1显示如下：

[ INFO] [1546309142.653888277]: Ready to add 3 ints.
[ INFO] [1546309198.935290286]: request:A=1,B=2 C=3
[ INFO] [1546309198.935315559]: sending back response:[6]
