---
title: ROS-Solidworks转URDF
date: 2019-1-21
categories:
- 笔记
tags:
- ros
toc: true
img: /medias/paperimg/ros.jpg

---
URDF建模很粗糙,而ros提供了支持sw转urdf的插件,可以使建模更精细化。<!-- more -->
# 一、安装sw_urdf_exporter插件

sw_urdf_exporter插件网址:[http://wiki.ros.org/sw_urdf_exporter](http://wiki.ros.org/sw_urdf_exporter)

安装时关闭sw,一路默认安装就可以了。

# 二、SW转URDF

使用SW打开将要转化的零件图。

![image](http://upload-images.jianshu.io/upload_images/16115686-f6fb60f43d62abb2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后打开sw_urdf_exporter插件 ![image](http://upload-images.jianshu.io/upload_images/16115686-c31ad2052bd9004f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

选择保存位置，名字使用**英文小写**命名，如part。点finish

![image](http://upload-images.jianshu.io/upload_images/16115686-7244f3a9e42d86b9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这时在保存路径上会生成功能包

![image](http://upload-images.jianshu.io/upload_images/16115686-09a478963072233c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 三、修改文件

## 3.1 将part.SLDPRT功能包原封不动放在工作空间的src内

 ![image](http://upload-images.jianshu.io/upload_images/16115686-88e2b7634e2a3a1f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##  3.2 修改文件（装配体）

修改package.xml文件

```
<maintainer email="me2email.com" />
修改为
<maintainer email="me2@email.com" />
```

修改两个launch文件

```
textfile="$(find my_paper_car)/robots/my_paper_car.urdf" />
修改为
textfile="$(find my_paper_car)/urdf/my_paper_car.urdf" />
```

## 3.3  新建launch文件（单零件）

在launch文件夹下新建display.launch文件,内容如下:

```
<launch>
    <param name="robot_description" textfile="$(find part.SLDPRT)/urdf/test.urdf" />
   
    <node name="rviz" pkg="rviz" type="rviz" args="-d $(find part.SLDPRT)/test.rviz"  />
</launch>
```

# 四、运行

启动dispaly.launch文件

并在rviz里add robomodel

![image](http://upload-images.jianshu.io/upload_images/16115686-816c15723b1a61ea.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 放大后效果如下:

![image](http://upload-images.jianshu.io/upload_images/16115686-a9a2f06e2c64b48e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
