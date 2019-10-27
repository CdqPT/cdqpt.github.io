---
title: python-opencv进阶应用
date: 2019-04-14 19:32:00
categories: 笔记
tags: 
- python
- opencv
toc: true
img: /medias/paperimg/python.jpg
summary: python学习笔记
author: 陈德强
top: 
password: 
cover: 
---
基础知识请参考[python-opencv安装及入门](https://purethought.cn/2019/04/14/%E7%BC%96%E7%A8%8B/Python/python-opencv%E5%AE%89%E8%A3%85%E5%8F%8A%E5%85%A5%E9%97%A8/)


墙裂推荐使用Pycharm编辑器，因为要靠他填坑！


# 一、颜色识别or轮廓提取
在 opencv 中颜色识别是最基础,应用最多的内容,一般来讲,在 opencv 中识别特定的颜色需要以下几个步骤:
1. 颜色空间转换,将 BGR 转化为 HSV 颜色空间,利用色调区别颜色
2. 按照阈值滤出所识别的颜色
3. 连续的开闭运算,消除噪点,平滑边界
4. 提取连通域,提取出要识别的颜色
开闭运算就是连续的腐蚀膨胀。
开运算:先腐蚀再膨胀,用来消除小物体
闭运算:先膨胀再腐蚀,用于排除小型黑洞

例程：识别红色和黄色组成的火焰,提取火轮廓。
```python
import cv2
import numpy as np
cap=cv2.VideoCapture(0)#创建视频窗口
red_min=np.array([0,128,46])#设置红色最小值
red_max=np.array([5,255,255])#设置红色最大值
red2_min=np.array([156,128,46])#设置红色2最小值
red2_max=np.array([180,255,255])#设置红色2最大值
yellow_min=np.array([15,128,46])#设置黄色最小值
yellow_max=np.array([50,255,255])#设置黄色最大值
while True:
	ret,frame=cap.read()#读取摄像头
	cv2.imshow('video',frame)#显示摄像头
	x,y=frame.shape[0:2]
	small_frame=cv2.resize(frame,(int(y/2),int(x/2)))
	cv2.imshow('small',small_frame)
	src=small_frame.copy()
	res=src.copy()
	hsv=cv2.cvtColor(src,cv2.COLOR_BGR2HSV)#转化为hsv
	cv2.imshow('hsv',hsv)



	#二值化，只提取出满足阈值的像素点
	mask_red1=cv2.inRange(hsv,red_min,red_max)
	mask_red2=cv2.inRange(hsv,red2_min,red2_max)
	mask_yellow=cv2.inRange(hsv,yellow_min,yellow_max)
	mask=cv2.bitwise_or(mask_red1,mask_red2)
	mask=cv2.bitwise_or(mask,mask_yellow)
	res=cv2.bitwise_and(src,src,mask=mask)
	h,w=res.shape[:2]
	blured=cv2.blur(res,(5,5))
	ret,bright=cv2.threshold(blured,10,255,cv2.THRESH_BINARY)
	gray=cv2.cvtColor(bright,cv2.COLOR_BGR2GRAY)
	#连续开闭运算
	kernel=cv2.getStructuringElement(cv2.MORPH_RECT,(5,5))
	opened=cv2.morphologyEx(gray,cv2.MORPH_OPEN,kernel)
	closed=cv2.morphologyEx(opened,cv2.MORPH_CLOSE,kernel)
	#提取连通域,得到全部红色黄色区间
	contours,hierarchy=cv2.findContours(closed,cv2.RETR_TREE,cv2.CHAIN_APPROX_SIMPLE)
	cv2.drawContours(src,contours,-1,(255,0,0),2)
	#提取火焰轮廓
	cv2.imshow('result',src)



	if cv2.waitKey(1) & 0xFF == ord('q'):
		break
cap.release()
cv2.destroyAllWindows()
```

# 二、形状提取
## 2.1 直线检测
直线检测可以通过 HoughLines 和 HoughLinesP 函数来完成,它们仅有的差别是:
第一个函数使用标准 Hough 变换,第二个函数使用概率 Hough 变换。HoughlinesP函数之所以称为概率版本的 Hough 变换是因为它只通过分析点的子集并估计这些点都属于一条直线的概率,这是标准 Hough 变换的优化版本。该函数计算代价少,执行更快。
```python
import cv2
import numpy as np
cap = cv2.VideoCapture(0)
while True:
	ret, frame = cap.read()
	x, y = frame.shape[0:2]
	small_frame = cv2.resize(frame, (int(y/2), int(x/2)))
	cv2.imshow('small', small_frame)
	gray = cv2.cvtColor(small_frame, cv2.COLOR_BGR2GRAY)
	edges = cv2.Canny(gray, 50, 120)
	minLineLength = 10
	maxLineGap = 5
	lines = cv2.HoughLinesP(edges, 1, np.pi/180, 100, minLineLength, maxLineGap)
	for x1, y1, x2, y2 in lines[0]:
		cv2.line(small_frame, (x1, y1), (x2, y2), (0, 255, 0), 2)
	cv2.imshow("lines", small_frame)
	if cv2.waitKey(1) & 0xFF == ord('q'):
		break
cap.release()
cv2.destroyAllWindows()
```
## 2.2 圆形检测
OpenCV 的 HoughCircles 函数可以用来检测圆,它与使用 HoughLines 函数类似。像用来决定删除或保留直线的两个参数 minLineLength 和 maxLineGap 一样,HoughCIrcles 有一个圆心间最小距离和圆的最小及最大半径。
```python
import cv2
import numpy as np
cap = cv2.VideoCapture(0)
while True:
	ret, frame = cap.read()
	x, y = frame.shape[0:2]
	small_frame = cv2.resize(frame, (int(y/2), int(x/2)))
	cv2.imshow('small', small_frame)
	gray = cv2.cvtColor(small_frame, cv2.COLOR_BGR2GRAY)
	gray_img = cv2.medianBlur(gray, 5)
	cimg = cv2.cvtColor(gray_img, cv2.COLOR_GRAY2BGR)
	circles = cv2.HoughCircles(gray_img, cv2.HOUGH_GRADIENT, 1, 120, param1=100,
	param2=30, minRadius=0, maxRadius=0)
	circles = np.uint16(np.around(circles))
	for i in circles[0,:]:
		cv2.circle(small_frame, (i[0], i[1]), i[2], (0, 255, 0), 2)
		cv2.circle(small_frame, (i[0], i[1]), 2, (0, 0, 255), 3)
	cv2.imshow("circles", small_frame)
	if cv2.waitKey(1) & 0xFF == ord('q'):
		break
cap.release()
cv2.destroyAllWindows()
```

## 2.3 检测其他形状
Opencv 中提供了 approxPloyDP 函数来检测所有形状,该函数提供多边形近似,如果您要检测的图形有多边形,结合 cv2.findContours 函数和 cv2.approxPloyDP 函数,可以相当准确的检测出来。
# 三、人脸识别
## 3.1 人脸数据采集
```python
import cv2
import numpy as np
import os
def generate_img(dirname):
	face_cascade=cv2.CascadeClassifier('/home/cdq/pythonStudy/auto/model/haarcascade_frontalface_default.xml')
	if (not os.path.isdir(dirname)):
		os.makedirs(dirname)
	cap = cv2.VideoCapture(0)
	count = 0
	while True:
		ret, frame = cap.read()
		x, y = frame.shape[0:2]
		small_frame = cv2.resize(frame, (int(y/2), int(x/2)))
		result = small_frame.copy()
		gray = cv2.cvtColor(small_frame, cv2.COLOR_BGR2GRAY)
		faces = face_cascade.detectMultiScale(gray, 1.3, 5)
		for (x, y, w, h) in faces:
			result = cv2.rectangle(result, (x, y), (x+w, y+h), (255, 0, 0), 2)
			f = cv2.resize(gray[y:y+h, x:x+w], (200, 200))
			if count<20:
				cv2.imwrite(dirname + '%s.pgm' % str(count), f)
				print(count)
				count += 1
		cv2.imshow('face', result)
		if cv2.waitKey(30) & 0xFF == ord('q'):
			break
	cap.release()
	cv2.destroyAllWindows()
if __name__ == '__main__':
	generate_img("/home/cdq/pythonStudy/auto/model/data/zhoujielun/")
```
```
解析：if __name__ == '__main__':
   一个python的文件有两种使用的方法，第一是直接作为脚本执行，第二是import到其他的python脚本中被调用（模块重用）执行。因此if __name__ == 'main': 的作用就是控制这两种情况执行代码的过程，在if __name__ == 'main': 下的代码只有在第一种情况下（即文件作为脚本直接执行）才会被执行，而import到其他脚本中是不会被执行的。
```


## 3.2 人脸识别

注意注意！！！！此代码要求python3.5环境！！！强烈建议使用Pycharm进行配置！！！
file-setting-project-interpret，然后在右上方的project interpret里选择Python3.5 /usr/bin/python3.5
如果置完还有错，就可能时缺少了opencv-contrib-python，这是在刚才的project interpret右下方选择加号，点击下方的manage repository，把里面默认的删掉，添加新的网址`https://pypi.python.org/simple`，点击确定后，就可以在搜索框里搜索并安装opencv-contrib-python了。配置环境真是害死人啊～

```python
import os
import sys
import cv2
import numpy as np
def read_images(path, sz=None):
	c = 0
	X, y = [], []
	names = []
	for dirname, dirnames, filenames in os.walk(path):
		for subdirname in dirnames:
			subject_path = os.path.join(dirname, subdirname)
			for filename in os.listdir(subject_path):
				try:
					if (filename == ".directory"):
						continue
					filepath = os.path.join(subject_path, filename)
					im = cv2.imread(os.path.join(subject_path, filename), cv2.IMREAD_GRAYSCALE)
					if (im is None):
						print("image" + filepath + "is None")
					if (sz is not None):
						im = cv2.resize(im, sz)
					X.append(np.asarray(im, dtype=np.uint8))
					y.append(c)
				except:
					print("unexpected error")
					raise
			c = c+1
			names.append(subdirname)
	return [names, X, y]
def face_rec():
	read_dir = "/home/cdq/pythonStudy/auto/model/data"
	[names, X, y] = read_images(read_dir)
	y = np.asarray(y, dtype=np.int32)
	model = cv2.face_EigenFaceRecognizer.create()
	model.train(np.asarray(X), np.asarray(y))
	face_cascade=cv2.CascadeClassifier('/home/cdq/pythonStudy/auto/model/haarcascade_frontalface_default.xml')
	cap = cv2.VideoCapture(0)
	while True:
		ret, frame = cap.read()
		x, y = frame.shape[0:2]
		small_frame = cv2.resize(frame, (int(y/2), int(x/2)))
		result = small_frame.copy()
		gray = cv2.cvtColor(small_frame, cv2.COLOR_BGR2GRAY)
		faces = face_cascade.detectMultiScale(gray, 1.3, 5)
		for (x, y, w, h) in faces:
			result = cv2.rectangle(result, (x, y), (x+w, y+h), (255, 0, 0), 2)
			roi = gray[x:x+w, y:y+h]
			try:
				roi = cv2.resize(roi, (200,200), interpolation=cv2.INTER_LINEAR)
				[p_label, p_confidence] = model.predict(roi)
				#print(names[p_label])
				cv2.putText(result, names[p_label], (x, y-20), cv2.FONT_HERSHEY_SIMPLEX,1, 255, 2)
			except:
				continue
		cv2.imshow("recognize_face", result)
		if cv2.waitKey(30) & 0xFF == ord('q'):
			break
	cap.release()
	cv2.destroyAllWindows()
if __name__ == "__main__":
	face_rec()
```
# 四、二维码识别
二维码识别物体,定位在机器人视觉领域是非常常见的功能。二维码识别共有两个步骤:
1. 找到二维码
2. 识别二维码所包含的内容

## 4.1 二维码定位

定位一张图片中的二维码需要以下几个小步骤:
1) 将图像转化为灰度图像
2) 使用 opencv 自带的 Sobel 算子进行过滤
Sobel 算子是一种常用的边缘检测算子,是一阶的梯度算法;
对噪声具有平滑作用,提供较为精确的边缘方向信息,边缘定位精度不够高;
当对精度要求不是很高时,是一种较为常用的边缘检测方法。
常见的应用和物理意义是边缘检测。
运行程序前的准备：
```
pip3 install pyzbar
```
```python
# coding:utf8

import cv2
import pyzbar.pyzbar as pyzbar


def decodeDisplay(image):
    barcodes = pyzbar.decode(image)
    for barcode in barcodes:
        # 提取条形码的边界框的位置
        # 画出图像中条形码的边界框
        (x, y, w, h) = barcode.rect
        cv2.rectangle(image, (x, y), (x + w, y + h), (0, 0, 255), 2)

        # 条形码数据为字节对象，所以如果我们想在输出图像上
        # 画出来，就需要先将它转换成字符串
        barcodeData = barcode.data.decode("utf-8")
        barcodeType = barcode.type

        # 绘出图像上条形码的数据和条形码类型
        text = "{} ({})".format(barcodeData, barcodeType)
        cv2.putText(image, text, (x, y - 10), cv2.FONT_HERSHEY_SIMPLEX,
                    .5, (0, 0, 125), 2)

        # 向终端打印条形码数据和条形码类型
        print("[INFO] Found {} barcode: {}".format(barcodeType, barcodeData))
    return image


def detect():
    camera = cv2.VideoCapture(0)

    while True:
        # 读取当前帧
        ret, frame = camera.read()
        # 转为灰度图像
        gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
        im = decodeDisplay(gray)

        cv2.waitKey(5)
        cv2.imshow("camera", im)

    camera.release()
    cv2.destroyAllWindows()


if __name__ == '__main__':
    detect()

```


# 五、目标追踪
camshift 利用目标的颜色直方图模型将图像转换为颜色概率分布图,初始化一个搜索窗的大小和位置,并根据上一帧得到的结果自适应调整搜索窗口的位置和大小,从而定位出当前图像中目标的中心位置。
tracker_base.py跟踪基类
```python
import cv2
import numpy as np
import time
class TrackerBase(object):
	def __init__(self, window_name):
		self.window_name = window_name
		self.frame = None
		self.frame_width = None
		self.frame_height = None
		self.frame_size = None
		self.drag_start = None
		self.selection = None
		self.track_box = None
		self.detect_box = None
		self.display_box = None
		self.marker_image = None
		self.processed_image = None
		self.display_image = None
		self.target_center_x = None
		self.cps = 0
		self.cps_values = list()
		self.cps_n_values = 20
	#####mouse event#####
	def onMouse(self, event, x, y, flags, params):
		if self.frame is None:
			return
		if event == cv2.EVENT_LBUTTONDOWN and not self.drag_start:
			self.track_box = None
			self.detect_box = None
			self.drag_start = (x, y)
		if event == cv2.EVENT_LBUTTONUP:
			self.drag_start = None
			self.detect_box = self.selection
		if self.drag_start:
			xmin = max(0, min(x, self.drag_start[0]))
			ymin = max(0, min(y, self.drag_start[1]))
			xmax = min(self.frame_width, max(x, self.drag_start[0]))
			ymax = min(self.frame_height, max(y, self.drag_start[1]))
			self.selection = (xmin, ymin, xmax-xmin, ymax-ymin)
	#####display selection box#####
	def display_selection(self):
		if self.drag_start and self.is_rect_nonzero(self.selection):
			x, y, w, h = self.selection
			cv2.rectangle(self.marker_image, (x, y), (x + w, y + h), (0,255, 255), 2)
	#####calculate if rect is zero#####
	def is_rect_nonzero(self, rect):
		try:
			(_,_,w,h) = rect
			return ((w>0)and(h>0))
		except:
			try:
				((_,_),(w,h),a) = rect
				return (w > 0) and (h > 0)
			except:
				return False
	#####rgb-image callback function#####
	def rgb_image_callback(self, data):
		start = time.time()
		frame = data
		if self.frame is None:
			self.frame = frame.copy()
			self.marker_image = np.zeros_like(frame)
			self.frame_size = (frame.shape[1], frame.shape[0])
			self.frame_width, self.frame_height = self.frame_size
			cv2.imshow(self.window_name, self.frame)
			cv2.setMouseCallback(self.window_name,self.onMouse)
			cv2.waitKey(3)
		else:
			self.frame = frame.copy()
			self.marker_image = np.zeros_like(frame)
			processed_image = self.process_image(frame)
			self.processed_image = processed_image.copy()
			self.display_selection()
			self.display_image = cv2.bitwise_or(self.processed_image,self.marker_image)
			###show track-box###
			if self.track_box is not None and self.is_rect_nonzero(self.track_box):
				tx, ty, tw, th = self.track_box
				cv2.rectangle(self.display_image, (tx, ty), (tx+tw,ty+th), (0, 0, 0), 2)
			###show detect-box###
			elif self.detect_box is not None and self.is_rect_nonzero(self.detect_box):
				dx, dy, dw, dh = self.detect_box
				cv2.rectangle(self.display_image, (dx, dy), (dx+dw,dy+dh), (255, 50, 50), 2)
				###calcate time and fps###
			end = time.time()
			duration = end - start
			fps = int(1.0/duration)
			self.cps_values.append(fps)
			if len(self.cps_values)>self.cps_n_values:
				self.cps_values.pop(0)
			self.cps = int(sum(self.cps_values)/len(self.cps_values))
			font_face = cv2.FONT_HERSHEY_SIMPLEX
			font_scale = 0.5
			if self.frame_size[0] >= 640:
				vstart = 25
				voffset = int(50+self.frame_size[1]/120.)
			elif self.frame_size[0] == 320:
				vstart = 15
				voffset = int(35+self.frame_size[1]/120.)
			else:
				vstart = 10
				voffset = int(20 + self.frame_size[1] / 120.)
			cv2.putText(self.display_image, "CPS: " + str(self.cps), (10,vstart), font_face, font_scale,(255, 255, 0))
			cv2.putText(self.display_image, "RES: " +str(self.frame_size[0]) + "X" + str(self.frame_size[1]), (10, voffset),font_face, font_scale, (255, 255, 0))
			###show result###
			cv2.imshow(self.window_name, self.display_image)
			cv2.waitKey(3)
	#####process image#####
	def process_image(self, frame):
		return frame
if __name__=="__main__":
	cap = cv2.VideoCapture(0)
	trackerbase = TrackerBase('base')
	while True:
		ret, frame = cap.read()
		x, y = frame.shape[0:2]
		small_frame = cv2.resize(frame, (int(y/2), int(x/2)))
		trackerbase.rgb_image_callback(small_frame)
		if cv2.waitKey(1) & 0xFF == ord('q'):
			break
	cap.release()
	cv2.destroyAllWindows()
```


注意：上一个文件要和此文件放在同一个目录下才能被找到。

camshift.py跟踪类
```python
#!/usr/bin/env python3
import cv2
import numpy as np
from tracker_base import TrackerBase
class Camshift(TrackerBase):
	def __init__(self, window_name):
		super(Camshift, self).__init__(window_name)
		self.detect_box = None
		self.track_box = None
	def process_image(self, frame):
		try:
			if self.detect_box is None:
				return frame
			src = frame.copy()
			if self.track_box is None or not self.is_rect_nonzero(self.track_box):
				self.track_box = self.detect_box
				x,y,w,h = self.track_box
				self.roi = cv2.cvtColor(frame[y:y+h, x:x+w], cv2.COLOR_BGR2HSV)
				roi_hist = cv2.calcHist([self.roi], [0], None, [16], [0, 180])
				self.roi_hist = cv2.normalize(roi_hist, roi_hist, 0, 255, cv2.NORM_MINMAX)
				self.term_crit = (cv2.TERM_CRITERIA_EPS | cv2.TERM_CRITERIA_COUNT, 10,1)
			else:
				hsv = cv2.cvtColor(frame,cv2.COLOR_BGR2HSV)
				back_project = cv2.calcBackProject([hsv],[0],self.roi_hist,[0,180],1)
				ret,self.track_box=cv2.CamShift(back_project,self.track_box,self.term_crit)
				pts = cv2.boxPoints(ret)
				pts = np.int0(pts)
				cv2.polylines(frame,[pts],True,255,1)
		except:
			pass
		return frame

if __name__ == '__main__':
	cap = cv2.VideoCapture(0)
	camshift = Camshift('camshift')
	while True:
		ret, frame = cap.read()
		x, y = frame.shape[0:2]
		small_frame = cv2.resize(frame, (int(y/2), int(x/2)))
		camshift.rgb_image_callback(small_frame)
		if cv2.waitKey(1) & 0xFF == ord('q'):
			break
	cap.release()
	cv2.destroyAllWindows()
```

# 六、光流法目标跟踪
光流的概念是 Gibson 在 1950 年首先提出来的。它是空间运动物体在观察成像平面上的像素运动的瞬时速度,是利用图像序列中像素在时间域上的变化以及相邻帧之间的相关性来找
到上一帧跟当前帧之间存在的对应关系,从而计算出相邻帧之间物体的运动信息的一种方法。一般而言,光流是由于场景中前景目标本身的移动、相机的运动,或者两者的共同运动所产生的。其计算方法可以分为三类:
(1)基于区域或者基于特征的匹配方法;
(2)基于频域的方法;
(3)基于梯度的方法;
简单来说,光流是空间运动物体在观测成像平面上的像素运动的“瞬时速度”。光流的研究是利用图像序列中的像素强度数据的时域变化和相关性来确定各自像素位置的“运动”。研究光流场的目的就是为了从图片序列中近似得到不能直接得到的运动场。
## 6.1 特征点提取


good_feature.py
```python
#!/usr/bin/env python3
import cv2
from tracker_base import TrackerBase
import numpy as np
class GoodFeatures(TrackerBase):
	def __init__(self, window_name):
		super(GoodFeatures, self).__init__(window_name)
		self.feature_size = 1
		# Good features parameters
		self.gf_maxCorners = 200
		self.gf_qualityLevel = 0.02
		self.gf_minDistance = 7
		self.gf_blockSize = 10
		self.gf_useHarrisDetector = True
		self.gf_k = 0.04
		# Store all parameters together for passing to the detector
		self.gf_params = dict(maxCorners = self.gf_maxCorners,qualityLevel = self.gf_qualityLevel,minDistance = self.gf_minDistance,blockSize = self.gf_blockSize,useHarrisDetector = self.gf_useHarrisDetector,k = self.gf_k)
		self.keypoints = list()
		self.detect_box = None
		self.mask = None
	def process_image(self, frame):
		try:
			if not self.detect_box:
				return frame
			src = frame.copy()
			gray = cv2.cvtColor(src, cv2.COLOR_BGR2GRAY)
			gray = cv2.equalizeHist(gray)
			keypoints = self.get_keypoints(gray, self.detect_box)
			if keypoints is not None and len(keypoints) > 0:
				for x, y in keypoints:
					cv2.circle(self.marker_image, (x, y), self.feature_size, (0, 255, 0), -1)
		except:
			pass
		return frame
	def get_keypoints(self, input_image, detect_box):
		self.mask = np.zeros_like(input_image)
		try:
			x, y, w, h = detect_box
		except:
			return None
		self.mask[y:y+h, x:x+w] = 255
		keypoints = list()
		kp = cv2.goodFeaturesToTrack(input_image, mask = self.mask, **self.gf_params)
		if kp is not None and len(kp) > 0:
			for x, y in np.float32(kp).reshape(-1, 2):
				keypoints.append((x, y))
		return keypoints
if __name__ == '__main__':
	cap = cv2.VideoCapture(0)
	goodfeatures = GoodFeatures('good_feature')
	while True:
		ret, frame = cap.read()
		x, y = frame.shape[0:2]
		small_frame = cv2.resize(frame, (int(y/2), int(x/2)))
		goodfeatures.rgb_image_callback(small_frame)
		if cv2.waitKey(1) & 0xFF == ord('q'):
			break
	cap.release()
	cv2.destroyAllWindows()
```
## 6.2 光流跟踪

lk_tracker.py

```python
#!/usr/bin/env python3
import cv2
import numpy as np
from good_features import GoodFeatures
class LKTracker(GoodFeatures):
	def __init__(self, window_name):
		super(LKTracker, self).__init__(window_name)
		self.feature_size = 1
		self.lk_winSize = (10, 10)
		self.lk_maxLevel = 2
		self.lk_criteria = (cv2.TERM_CRITERIA_EPS | cv2.TERM_CRITERIA_COUNT, 20, 0.01)
		self.lk_params = dict( winSize = self.lk_winSize,maxLevel = self.lk_maxLevel,criteria = self.lk_criteria)
		self.detect_interval = 1
		self.keypoints = None
		self.detect_box = None
		self.track_box = None
		self.mask = None
		self.gray = None
		self.prev_gray = None
	def process_image(self, frame):
		try:
			if self.detect_box is None:
				return frame
			src = frame.copy()
			self.gray = cv2.cvtColor(src, cv2.COLOR_BGR2GRAY)
			self.gray = cv2.equalizeHist(self.gray)
			if self.track_box is None or not self.is_rect_nonzero(self.track_box):
				self.track_box = self.detect_box
				self.keypoints = self.get_keypoints(self.gray, self.track_box)
			else:
				if self.prev_gray is None:
					self.prev_gray = self.gray
				self.track_box = self.track_keypoints(self.gray, self.prev_gray)
			self.prev_gray = self.gray
		except:
			pass
		return frame
	def track_keypoints(self, gray, prev_gray):
		img0, img1 = prev_gray, gray
		# Reshape the current keypoints into a numpy array required by calcOpticalFlowPyrLK()
		p0 = np.float32([p for p in self.keypoints]).reshape(-1, 1, 2)
		# Calculate the optical flow from the previous frame to the current frame
		p1, st, err = cv2.calcOpticalFlowPyrLK(img0, img1, p0, None, **self.lk_params)
		try:
			# Do the reverse calculation: from the current frame to the previous frame
			p0r, st, err = cv2.calcOpticalFlowPyrLK(img1, img0, p1, None, **self.lk_params)
			# Compute the distance between corresponding points in the two flows
			d = abs(p0-p0r).reshape(-1, 2).max(-1)
			# If the distance between pairs of points is < 1 pixel, set a value in the "good" array to True, otherwise False
			good = d<1
			new_keypoints = list()
			for(x, y), good_flag in zip(p1.reshape(-1, 2), good):
				if not good_flag:
					continue
				new_keypoints.append((x, y))
				cv2.circle(self.marker_image, (x, y), self.feature_size, (255, 255, 0), -1)
			self.keypoints = new_keypoints
			# Convert the keypoints list to a numpy array
			keypoints_array = np.float32([p for p in self.keypoints]).reshape(-1, 1, 2)
			if len(self.keypoints)>6:
				track_box = cv2.boundingRect(keypoints_array)
			else:
				track_box = cv2.boundingRect(keypoints_array)
		except:
			track_box = None
		return track_box
if __name__ == '__main__':
	cap = cv2.VideoCapture(0)
	lk_tracker = LKTracker('lk_tracker')
	while True:
		ret, frame = cap.read()
		x, y = frame.shape[0:2]
		small_frame = cv2.resize(frame, (int(y/2), int(x/2)))
		lk_tracker.rgb_image_callback(small_frame)
		if cv2.waitKey(1) & 0xFF == ord('q'):
			break
	cap.release()
	cv2.destroyAllWindows()
```
# 七、KCF+Kalman 目标跟踪
KCF 是一种鉴别式追踪方法,这类方法一般都是在追踪过程中训练一个目标检测器,使用目标检测器去检测下一帧预测位置是否是目标,然后再使用新检测结果去更新训练集进而更新目标检测器。而在训练目标检测器时一般选取目标区域为正样本,目标的周围区域为负样本,当然越靠近目标的区域为正样本的可能性越大。
Kcf 的主要贡献:
使用目标周围区域的循环矩阵采集正负样本,利用脊回归训练目标检测器,并成功的利用循环矩阵在傅里叶空间可对角化的性质将矩阵的运算转化为向量的 Hadamad 积,即元素的点乘,大大降低了运算量,提高了运算速度,使算法满足实时性要求。将线性空间的脊回归通过核函数映射到非线性空间,在非线性空间通过求解一个对偶问题和某些常见的约束,同样的可以使用循环矩阵傅里叶空间对角化简化计算。给出了一种将多通道数据融入该算法的途径。

注意：如果提示缺少模块，请安装
```
pip3 install opencv-contrib-python
```

kcf_kalman_tracker.py
```python
#!/usr/bin/env python3
import cv2
import numpy as np
from tracker_base import TrackerBase
class KcfKalmanTracker(TrackerBase):
	def __init__(self, window_name):
		super(KcfKalmanTracker, self).__init__(window_name)
		self.tracker = cv2.TrackerKCF_create()
		self.detect_box = None
		self.track_box = None
		####init kalman####
		self.kalman = cv2.KalmanFilter(4, 2)
		self.kalman.measurementMatrix = np.array([[1,0,0,0],[0,1,0,0]],np.float32)
		self.kalman.transitionMatrix=np.array([[1,0,1,0],[0,1,0,1],[0,0,1,0],[0,0,0,1]],np.float32)
		self.kalman.processNoiseCov=np.array([[1,0,0,0],[0,1,0,0],[0,0,1,0],[0,0,0,1]],np.float32)*0.03
		self.measurement = np.array((2,1),np.float32)
		self.prediction = np.array((2,1),np.float32)
	def process_image(self, frame):
		try:
			if self.detect_box is None:
				return frame
			src = frame.copy()
			if self.track_box is None or not self.is_rect_nonzero(self.track_box):
				self.track_box = self.detect_box
				if self.tracker is None:
					raise Exception("tracker not init")
				status = self.tracker.init(src, self.track_box)
				if not status:
					raise Exception("tracker initial failed")
			else:
				self.track_box = self.track(frame)
		except:
			pass
		return frame
	def track(self, frame):
		status, coord = self.tracker.update(frame)
		center=np.array([[np.float32(coord[0]+coord[2]/2)],[np.float32(coord[1]+coord[3]/2)]])
		self.kalman.correct(center)
		self.prediction = self.kalman.predict()
		cv2.circle(frame, (int(self.prediction[0]),int(self.prediction[1])),4,(255,60,100),2)
		round_coord = (int(coord[0]), int(coord[1]), int(coord[2]), int(coord[3]))
		return round_coord
if __name__ == '__main__':
	cap = cv2.VideoCapture(0)
	kcfkalmantracker = KcfKalmanTracker('base')
	while True:
		ret, frame = cap.read()
		x, y = frame.shape[0:2]
		small_frame = cv2.resize(frame, (int(y/2), int(x/2)))
		kcfkalmantracker.rgb_image_callback(small_frame)
		if cv2.waitKey(1) & 0xFF == ord('q'):
			break
	cap.release()
	cv2.destroyAllWindows()
```

下一篇：[python-opencv高级应用](https://purethought.cn/2019/04/15/%E7%BC%96%E7%A8%8B/Python/python-opencv%E9%AB%98%E7%BA%A7%E5%BA%94%E7%94%A8/)