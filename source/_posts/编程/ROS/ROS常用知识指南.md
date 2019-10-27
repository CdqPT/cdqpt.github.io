---
title: ROS常用知识指南
date: 2019-1-2
categories:
- 笔记
tags:
- ros
toc: true
img: /medias/paperimg/ros.jpg

---
介绍一些基础常用的知识。<!-- more -->
# 一、标准单位

![image](http://upload-images.jianshu.io/upload_images/16115686-49a786db97cce0bb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# **二、坐标表现方式**

**![image](http://upload-images.jianshu.io/upload_images/16115686-eec7c3c0f66e3095.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)** 

# 三、默认安装位置

通过apt-get安装的软件包， 默认安装位置为：/opt/ros/kinetic/share

# 四、软件包安装流程

## 4.1 二进制安装（第一种方法）

如果想安装ROS功能包,可以用下面的apt-cache命令搜索以ros-kinetic开头的所有的功能包。利用这个命令可以搜索到大约1600多个功能包

```
apt-cache search ros-kinetic
```

使用以下命令安装软件包

```
sudo apt-get install ros-kinetic-[功能包名称]
```

## 4.2 源码编译（第二种方法）

**更新软件库**

```
sudo apt update
```


低于16.04版本的ubuntu输入：

```
sudo apt-get update
```

**安装依赖**

```
sudo apt-get install -y google-mock libboost-all-dev
```

注：-y后部分替换为自己需要安装的依赖，两个以上的依赖中间用空格隔开。

**安装软件包**

```
cd到需要编译的位置
git网址
编译
```

# 五、如何修改带有权限的文件

```
sudo gedit [文件名]
或
chmod +x 【文件名】
```

#  六、.py文件

.py文件一般会有权限,导致出现找不到软件包的错误提示;

这时需要cd到软件包目录,然后运行chmod +x [文件名]

赋予权限即可.
