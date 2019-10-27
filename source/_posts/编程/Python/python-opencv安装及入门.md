---
title: python-opencv安装及入门
date: 2019-04-14 15:39:00
categories: 教程
tags: 
- python
- opencv
toc: true
img: /medias/paperimg/python.jpg
summary: python入门笔记
author: 陈德强
top: 
password: 
cover: 
---
# 零、安装Python
[参考此文](https://purethought.cn/2019/04/14/%E5%AE%89%E8%A3%85%E5%8F%8A%E6%95%99%E7%A8%8B/python%E5%AE%89%E8%A3%85%E6%95%99%E7%A8%8B/)
# 一、安装python-opencv
安装pip
```
sudo apt install python-pip
```
安装opencv-python
前提是python3，可打开thonny编辑器看输出拦的提示，一般新版系统都是python3
```
pip3 install opencv-python
sudo apt-get install libatlas3-base
sudo apt-get install libjasper1
sudo apt-get install libgst7
sudo apt-get install python3-gst-1.0
sudo apt-get install libqtgui4
sudo apt-get install libqt4-test
sudo apt-get install libilmbase12
sudo apt-get install openexr
sudo apt-get install libavcodec57
sudo apt-get install libavformat57
sudo apt-get install libswscale4
```

# 二、测试程序-调用摄像头
新建vedio.py文件
```python
import cv2 #导入opencv库
cap = cv2.VideoCapture(0) #调用摄像头，参数是设备编号
#主循环是读取摄像头图像，按q停止
while True:
	ret, frame = cap.read()
	cv2.imshow('video',frame)
	if cv2.waitKey(1) & 0xFF == ord('q'):
		break
cap.release() #释放cap 
cv2.destroyAllWindows()#销毁所有窗口
```

-------------------------------------------
此时出现了BUG：
/home/gordon/python-virtual-environments/RL_2018HW/gym-gazebo:/home/gordon/ros_ws/devel/lib/python2.7/dist-packages:/opt/ros/kinetic/lib/python2.7/dist-packages:/opt/movidius/caffe/python
解决方案：
打开bash.rc文件，注释这一条
```
#source /opt/ros/kinetic/setup.bash
```
移除错误路径：
打开控制台
```
python 
```
Python 3.5.2 (default, Nov 23 2017, 16:37:01) 
[GCC 5.4.0 20160609] on linux
Type "help", "copyright", "credits" or "license" for more information.
```
import cv2
```
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ImportError: /opt/ros/kinetic/lib/python2.7/dist-packages/cv2.so: undefined symbol: PyCObject_Type
```
import sys
```
```
sys.path.remove('/opt/ros/kinetic/lib/python2.7/dist-packages')
```
```
import cv2
```
注销登录即可。

-------------------------------------------------------

再次运行上述程序，就会调用摄像头了！
# 三、颜色空间转换
OpenCV 中有数百种不同色彩空间转换的方法。当前最主要的,最常用的有三种颜色空间:灰度,BGR,HSV。
⚫ 灰度色彩空间是通过去除彩色信息来将其转换为灰阶,灰度色彩空间对中间处理特别有效,比如人脸检测。
⚫ BGR,即蓝-绿-红色彩空间,每一个像素点由一个三元数来表示,分别代表蓝绿红三种颜色
⚫ HSV,H(Hue)是色调,用角度度量,取值范围为 0°~360°,从红色开始按逆时针方向计算,红色为 0°,绿色为 120°,蓝色为 240°,S(Saturation)是饱和度,V(Value)表示黑暗程度。

```python
import cv2
cap = cv2.VideoCapture(0)
while True:
	ret, frame = cap.read()
	cv2.imshow('video',frame)
	#灰度转化
	gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
	cv2.imshow('gray',gray)
	#hsv转化
	hsv = cv2.cvtColor(frame, cv2.COLOR_BGR2HSV)
	cv2.imshow('hsv', hsv)
	if cv2.waitKey(1) & 0xFF == ord('q'):
		break
cap.release()
cv2.destroyAllWindows()
```
# 四、图像缩放
很多时候,读取图像过于清晰,会导致像素点过多,读取缓慢,影响效率,下面介绍 Opencv是如何在保留最大信息的前提下缩放图像的。
```python
import cv2
import numpy as np
cap = cv2.VideoCapture(0)
while True:
	ret, frame = cap.read()
	cv2.imshow('video',frame)
	#第 8 行, shape ()函数返回一个列表,这个列表第一个元素是图像宽度,第二个元素是图像高度,分别赋值为 x,y
	x, y = frame.shape[0:2]
	#第 9 行，resize 函数,将图像缩小为原图 1/4 大小，x/2,y/2
	small_frame = cv2.resize(frame, (int(y/2), int(x/2)))
	cv2.imshow('small', small_frame)
	if cv2.waitKey(1) & 0xFF == ord('q'):
		break
cap.release()
cv2.destroyAllWindows()
```
# 五、图像滤波
在 Opencv 中,对图像和视频的处理大多会涉及傅里叶变换,即一切波形都可以由一系列简单且频率不同的正弦曲线叠加得到。这个概念对图像处理非常有帮助,这样我们可以区分图像哪些区域信号变化特别强,哪些没那么强,从而可以任意标记噪声区域,感兴趣区域等。
⚫ 高通滤波:检测图像某个区域,根据像素与周围像素亮度差值来提升该亮度的滤波器
⚫ 低通滤波:在像素与周围像素亮度差值小于特定值时,平滑亮度,用于去噪和模糊化。
```python
import cv2
import numpy as np
cap = cv2.VideoCapture(0)
while True:
	ret, frame = cap.read()
	#cv2.imshow('video',frame)
	x, y = frame.shape[0:2]
	small_frame = cv2.resize(frame, (int(y/2), int(x/2)))
	cv2.imshow('small', small_frame)
	#Blur:模糊滤波
	img_mean = cv2.blur(small_frame, (5,5))
	#Gaussianblur:高斯滤波
	img_Guassian = cv2.GaussianBlur(small_frame, (5,5), 0)
	#Median:中值滤波
	img_median = cv2.medianBlur(small_frame, 5)
	#Bilater:双边滤波
	img_bilater = cv2.bilateralFilter(small_frame, 9, 75, 75)
	cv2.imshow('mean', img_mean)
	cv2.imshow('guassian', img_Guassian)
	cv2.imshow('median', img_median)
	cv2.imshow('bilater', img_bilater)
	if cv2.waitKey(1) & 0xFF == ord('q'):
		break
cap.release()
cv2.destroyAllWindows()
```

下一篇：[python-opencv进阶应用](https://purethought.cn/2019/04/14/%E7%BC%96%E7%A8%8B/Python/python-opencv%E8%BF%9B%E9%98%B6%E5%BA%94%E7%94%A8/)