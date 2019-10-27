---
title: 人脸检测（OpenCV版）
date: 2019-07-21 14:27:00
categories: 笔记
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

首先下载训练集，训练集的下载网址：[https://github.com/opencv/opencv](https://github.com/opencv/opencv)

# 一、图片人脸识别

```python
import cv2

filepath = "/Users/cdq/Desktop/aa.jpg"  #这里改成识别图片位置
img = cv2.imread(filepath)  # 读取图片
gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)  # 转换灰色

# OpenCV人脸识别分类器，这里改成训练集的位置
classifier = cv2.CascadeClassifier(
    "/Users/cdq/Documents/PythonFile/opencv-master/data/haarcascades/haarcascade_frontalface_default.xml"
)
color = (0, 255, 0)  # 定义绘制颜色
# 调用识别人脸
faceRects = classifier.detectMultiScale(
    gray, scaleFactor=1.2, minNeighbors=3, minSize=(32, 32))
if len(faceRects):  # 大于0则检测到人脸
    for faceRect in faceRects:  # 单独框出每一张人脸
        x, y, w, h = faceRect
        # 框出人脸
        cv2.rectangle(img, (x, y), (x + h, y + w), color, 2)
        # 左眼
        cv2.circle(img, (x + w // 4, y + h // 4 + 30), min(w // 8, h // 8),
                   color)
        #右眼
        cv2.circle(img, (x + 3 * w // 4, y + h // 4 + 30), min(w // 8, h // 8),
                   color)
        #嘴巴
        cv2.rectangle(img, (x + 3 * w // 8, y + 3 * h // 4),
                      (x + 5 * w // 8, y + 7 * h // 8), color)

cv2.imshow("image", img)  # 显示图像
c = cv2.waitKey(10)

cv2.waitKey(0)
cv2.destroyAllWindows()
```

# 二、视频人脸识别


```python
# -*- coding:utf-8 -*-
# OpenCV版本的视频检测
import cv2


# 图片识别方法封装，路径改一下
def discern(img):
    gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
    cap = cv2.CascadeClassifier(
        "/Users/cdq/Documents/PythonFile/opencv-master/data/haarcascades/haarcascade_frontalface_default.xml"
    )
    faceRects = cap.detectMultiScale(
        gray, scaleFactor=1.2, minNeighbors=3, minSize=(50, 50))
    if len(faceRects):
        for faceRect in faceRects:
            x, y, w, h = faceRect
            cv2.rectangle(img, (x, y), (x + h, y + w), (0, 255, 0), 2)  # 框出人脸
    cv2.imshow("Image", img)


# 获取摄像头0表示第一个摄像头
cap = cv2.VideoCapture(0)
while (1):  # 逐帧显示
    ret, img = cap.read()
    # cv2.imshow("Image", img)
    discern(img)
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break
cap.release()  # 释放摄像头
cv2.destroyAllWindows()  # 释放窗口资源
```

转载自：[https://github.com/vipstone/faceai](https://github.com/vipstone/faceai)