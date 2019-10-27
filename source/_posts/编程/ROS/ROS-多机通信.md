---
title: ROS-多机通信
date: 2017-11-16
categories:
- 笔记
tags:
- ros
toc: true
img: /medias/paperimg/ros.jpg

---

一定要在同一路由的局域网下进行,就是两台电脑的ip要像这样:192.168.191.4和192.168.191.8,只有最后一位不同,这样就能ping通了,否则ping不同。<!--more-->

# **一、查看ip和主机名**

### 1.1 查看ip:

```
ifconfig 
```

-----------------------------------------

显示如下：

**![image](http://upload-images.jianshu.io/upload_images/16115686-84547e3ce5e95efa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)** 

其中,lo是网线连接情况下的id,下边的wlp2s0是无线连接情况下的ip地址.

分别查看两台电脑的ip地址并用笔记本记录下来.一定要在同一局域网下进行!

### 1.2 查看主机名

```
hostname
```

分别查看两台电脑的ip地址并用记录下来.

这里说明一下,如果在"关于这台计算机"的选项里修改了名字后,导致没有了管理员权限,每次打开终端都会提示:To run a command.......

遇到这种情况,输入如下代码可以解决:

```
touch ~/.sudo as admin successful
```

# 二、修改host文件

### 2.1 打开host文件:

```
gksu gedit /etc/hosts
```

### 2.2 修改host文件

按照如下形式分别在两台电脑上添加ip和主机名：

![image](http://upload-images.jianshu.io/upload_images/16115686-255fed8bbe921163.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 2.3 重启网络：

两台电脑都需要重启网络。

```
sudo /etc/init.d/networking restart  
```

# 三、实现通信

### 3.1 在两台电脑上装上chrony包:

```
sudo apt-get install chrony 
```

### 3.2 在两台电脑上都安装ssh服务器:

```
sudo apt-get install openssh-server 
```

### 3.3 确认服务器是否已经启动：

```
ps -e|grep ssh 
```

如果看到sshd则说明ssh-server已经启动.

### 3.4 检测是否双向连通:

先ssh自己的主机名: 

```
ssh one_name
```
然后ping另一台电脑的主机名:

```
ping tow_name
```

-----------------------------------------

显示如下：

![image](http://upload-images.jianshu.io/upload_images/16115686-d6b1a255cdf4fca4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240) 

 接着在另一台机器操作:

```
ssh tow_name
ping one_name
```

-----------------------------------------

显示如下：

![image](http://upload-images.jianshu.io/upload_images/16115686-724c73415da23128.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如果两台机器都出现如上结果,说明双向连通,只要有一个没出现下边的结果就是没有连接成功,而多级通信必须保证双向连通.

### 3.5 修改.bashrc文件

在两台电脑上都使用下面的命令来编辑.bashrc文件：

```
gedit ~/.bashrc
```

在A端这边的bashrc文件的最后添加：

```
export ROS_HOSTNAME=one_hostname  
export ROS_MASTER_URI=http://one_hostname:11311  
```

-----------------------------------------

解析：

第一条是本机的主机名

第二条 是主机,也就是要运行roscore节点的电脑端的主机名

在B端这边的bashrc文件的最后添加：

```
export ROS_HOSTNAME=tow_hostname  
export ROS_MASTER_URI=http://one_hostname:11311 
```

注意：两台机器的export ROS_MASTER_URI=http://one_hostname:11311这条是一样的.

# 四、验证

### 4.1 电脑A端

首先在主机,启动 ROS：$ roscore 然后运行：

```
rosrun turtlesim turtlesim_node
```

### 4.2 电脑B端

```
rosrun turtlesim turtle_teleop_key 
```

现在,你可以在电脑B端控制A上的小乌龟移动啦!
