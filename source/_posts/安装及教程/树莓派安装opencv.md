---
title: 树莓派安装opencv
date: 2019-05-05 12:57:00
categories: 教程
tags: 嵌入式
toc: true
img: /medias/paperimg/mcu.jpg
summary: 手动编译树莓派安装opencv
author: 陈德强
top: 
password: 
cover: 
---

# 一、python3安装opencv
```
sudo pip3 install opencv-python
sudo pip3 install opencv-contrib-python
```

安装libhdf5动态库
```
sudo apt-get update
sudo apt-get install libhdf5-dev libhdf5-serial-dev
```
安装libQtGui.so动态库
```
sudo apt-get update
sudo apt install libqtgui4
```


# 二、python2安装opencv
这个手动编译超慢，几个小时，因此条件允许的话，使用python3吧。
更新软件
```
// 软件源更新
sudo apt-get update 
// 升级本地所有安装包，最新系统可以不升级，版本过高反而需要降级才能安装
sudo apt-get upgrade
// 升级树莓派固件，固件比较新或者是Ubuntu则不用执行
sudo rpi-update
```
安装构建OpenCV的相关工具
```
// 安装build-essential、cmake、git和pkg-config
sudo apt-get install build-essential cmake git pkg-config
```
安装常用图像工具包
```
// 安装jpeg格式图像工具包
sudo apt-get install libjpeg8-dev 
// 安装tif格式图像工具包
sudo apt-get install libtiff5-dev 
// 安装JPEG-2000图像工具包
sudo apt-get install libjasper-dev 
// 安装png图像工具包
sudo apt-get install libpng12-dev 
```
安装视频I/O包
```
sudo apt-get install libavcodec-dev libavformat-dev libswscale-dev libv4l-dev
```
安装gtk2.0
```
sudo apt-get install libgtk2.0-dev
```
优化函数包
```
sudo apt-get install libatlas-base-dev gfortran
```
编译OpenCV源代码
```
// 使用wget下载OpenCV源码，觉得慢的话可以到https://github.com/opencv/opencv/releases下载OpenCV的源代码
// tar.gz格式，需要解压好，放到用户目录下
// 但是OpenCV_contrib请大家使用wget，亲测直接到Github下载zip文件的话，会有编译问题

// 下载OpenCV
wget -O opencv-3.4.1.zip https://github.com/Itseez/opencv/archive/3.4.1.zip
// 解压OpenCV
unzip opencv-3.4.1.zip
// 下载OpenCV_contrib库：
wget -O opencv_contrib-3.4.1.zip https://github.com/Itseez/opencv_contrib/archive/3.4.1.zip
// 解压OpenCV_contrib库：
unzip opencv_contrib-3.4.1.zip
```

找到你下载的源码文件夹并打开，tar.gz解压后文件夹名应该是opencv-3.4.1（版本号可能会变化），git方式下载的文件夹名应该是opencv。之后我们新建一个名为release的文件夹用来存放cmake编译时产生的临时文件：
```
// 打开源码文件夹，这里以我修改文章时最新的3.4.1为例
cd opencv-3.4.1
// 新建release文件夹
mkdir release
// 进入release文件夹
cd release
```
设置cmake编译参数，安装目录默认为/usr/local ，注意参数名、等号和参数值之间不能有空格，但每行末尾“\”之前有空格，参数值最后是两个英文的点：
```
/** CMAKE_BUILD_TYPE是编译方式
* CMAKE_INSTALL_PREFIX是安装目录
* OPENCV_EXTRA_MODULES_PATH是加载额外模块
* INSTALL_PYTHON_EXAMPLES是安装官方python例程
* BUILD_EXAMPLES是编译例程（这两个可以不加，不加编译稍微快一点点，想要C语言的例程的话，在最后一行前加参数INSTALL_C_EXAMPLES=ON \）
**/

sudo cmake -D CMAKE_BUILD_TYPE=RELEASE \
    -D CMAKE_INSTALL_PREFIX=/usr/local \
    -D OPENCV_EXTRA_MODULES_PATH=~/opencv_contrib-3.4.1/modules \
    -D INSTALL_PYTHON_EXAMPLES=ON \
    -D BUILD_EXAMPLES=ON ..
```
开始正式编译
```
// 编译，以管理员身份，否则容易出错
sudo make
// 安装
sudo make install
// 更新动态链接库
sudo ldconfig
```
查看安装版本
```
pkg-config --modversion opencv
```
 
参考文献：
1.[树莓派机器视觉环境搭建](https://blog.csdn.net/weixin_42704669/article/details/84026895)
2.[树莓派安装OpenCV](https://www.jianshu.com/p/b75936848a27)