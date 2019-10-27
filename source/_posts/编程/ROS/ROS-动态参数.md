---
title: ROS-动态参数
date: 2019-1-1
categories:
- 笔记
tags:
- ros
toc: true
img: /medias/paperimg/ros.jpg

---
在节点外部改变参数的方式有：参数服务器、服务、主题以及动态参数。<!-- more -->
## 一、新建cfg文件

在chapter2_tutorials包下新建cfg文件夹，在cfg文件夹下新建chapter2.cfg文件，添加如下代码：
 ```python
#!/usr/bin/env python
PACKAGE = "chapter2_tutorials"

from dynamic_reconfigure.parameter_generator_catkin import *

gen = ParameterGenerator()  

gen.add("int_param", int_t, 0, "int parameter", 1, 0, 10)  
gen.add("double_param", double_t, 0, "double parameter", .1, 0.0, 1.0)
gen.add("bool_param", bool_t, 0, "bool parameter", True)
gen.add("str_param", str_t, 0, "string parameter", "chapter2_tutorials")

size_enum = gen.enum([ gen.const("Low",      int_t, 0, "Low is 0"),
                       gen.const("Medium",     int_t, 1, "Medium is 1"),
                       gen.const("High",      int_t, 2, "Hight is 2")],
                     "Select from the list")

gen.add("size", int_t, 0, "Select from the list", 1, 0, 3, edit_method=size_enum)

exit(gen.generate(PACKAGE, "chapter2_tutorials", "chapter2"))
```


其中：
**gen = ParameterGenerator()**  #创建一个动态参数生成器，后面就可以往该变量中添加可以动态配置的参数；

**gen.add("int_param", int_t, 0, "int parameter", 1, 0, 10)**  #（参数名，数据类型，回调函数掩码，参数说明，默认值，最小值，最大值）；

**exit(gen.generate(PACKAGE, "chapter2_tutorials", "chapter2"))**    #用于生成头文件并退出程序（？，包名，头文件名称）。

吐槽：

不得不吐槽，《ROS机器人高效编程（原书第3版）》在最后一行“chapter2_”这里多了个"_",导致编译时总是找不到头文件！

## 二、获取权限

因为文件由ros自动执行，所以需要使用chmod命令使文件可由任何用户执行

```
chmod a+x cfg/chapter2.cfg
```

## 三、修改Cmakelist.txt文件

```
find_package(catkin REQUIRED COMPONENTS
 message_generation 
 roscpp 
 std_msgs 
 dynamic_reconfigure)


 generate_dynamic_reconfigure_options(
   cfg/chapter2.cfg
      )
```

## 四、新建.cpp文件

在chapter2_tutorials包下新建example4.cpp文件，添加如下代码：

```c++
#include "ros/ros.h"
#include <dynamic_reconfigure/server.h>
#include <chapter2_tutorials/chapter2Config.h>
 
void callback(chapter2_tutorials::chapter2Config &config, uint32_t level)
{
    ROS_INFO("Reconfigure Request: %d %f %s %s %d",
        config.int_param,
        config.double_param,
        config.str_param.c_str(),
        config.bool_param?"True":"False",
        config.size);
}
 
int main(int argc, char **argv)
{
    ros::init(argc, argv, "examole5_dynamic_reconfigure");
    dynamic_reconfigure::Server<chapter2_tutorials::chapter2Config> server;
    dynamic_reconfigure::Server<chapter2_tutorials::chapter2Config>::CallbackType f;
    f = boost::bind(&callback, _1, _2);
    server.setCallback(f);
    ros::spin();
    return 0;
}
```

## 五、编译
## 六、运行
```
roscore
rosrun chapter2_tutorials example41
rosrun rqt_reconfigure rqt_reconfigure
```


显示如下：

![image](http://upload-images.jianshu.io/upload_images/16115686-72a6da170505d808.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

当调节滑动条后，显示显示如下：

马赛克$ rosrun chapter2_tutorials example41
[ INFO] [1546338804.944767450]: Reconfigure Request: 1 0.100000 chapter2_tutorials True 1
[ INFO] [1546338822.578500410]: Reconfigure Request: 1 0.370000 chapter2_tutorials True 1
[ INFO] [1546338824.890332107]: Reconfigure Request: 2 0.370000 chapter2_tutorials True 1
