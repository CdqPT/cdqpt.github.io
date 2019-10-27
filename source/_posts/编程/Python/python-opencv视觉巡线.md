---
title: python-opencv视觉巡线
date: 2019-04-20 17:20:00
categories: 笔记
tags: 
- python
- opencv
toc: true
img: /medias/paperimg/python.jpg
summary: python-opencv视觉巡线
top: 
password: 
cover: 
---

**简要概述：**

通过摄像头采集图像，
将图像灰度化、二值化、膨胀、腐蚀操作后，
提取第400行像素值v,接近于图像底线位置，
提取中间值（这里为白色）的数量和位置，
根据数量和位置，利用简单的数学公式，（首项+尾项）/2，计算出白色的中间位置，
然后对比实际的中间位置320（不需要改），计算出偏移量，
最后根据偏移量计算出电机应有的转角。

# 一、边缘检测实验

```python
#!/usr/bin/env python3

# 识别的是中线为白色

import cv2
import numpy as np

# center定义
center = 320
# 打开摄像头，图像尺寸640*480（长*高），opencv存储值为480*640（行*列）
cap = cv2.VideoCapture(0)
while (1):
    ret, frame = cap.read()
    cv2.imshow("recognize_face", frame)
    # 转化为灰度图
    gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
    cv2.imshow("gray", gray)
    # 大津法二值化
    retval, dst = cv2.threshold(gray, 0, 255, cv2.THRESH_OTSU)
    cv2.imshow("dst", dst)
    # 膨胀，白区域变大
    dst = cv2.dilate(dst, None, iterations=2)
    cv2.imshow("dst2", dst)
    # # 腐蚀，白区域变小 #
    dst = cv2.erode(dst, None, iterations=6)
    cv2.imshow("dst3", dst)
    # 单看第400行的像素值v
    color = dst[400]
    try:
        # 找到白色的像素点个数，如寻黑色，则改为0
        white_count = np.sum(color == 255)
        # 找到白色的像素点索引，如寻黑色，则改为0
        white_index = np.where(color == 255)
        # 防止white_count=0的报错
        if white_count == 0:
            white_count = 1
        # 找到黑色像素的中心点位置
        # 计算方法应该是边缘检测，计算白色边缘的位置和/2，即是白色的中央位置。
        center = (white_index[0][white_count - 1] + white_index[0][0]) / 2
        # 计算出center与标准中心点的偏移量，因为图像大小是640，因此标准中心是320，因此320不能改。
        direction = center - 320
        print(direction)
    except:
        continue
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break
cap.release() #释放cap
cv2.destroyAllWindows()#销毁所有窗口
```

# 二、树莓派GPIO应用

```python
# coding:utf-8 
# 实现树莓派小车的变速控制 
import RPi.GPIO as gpio 
# 定义引脚 
in1 = 12 
in2 = 16 
in3 = 18 
in4 = 22 
# 设置GPIO口为BOARD编号规范,从左到右，从上到下。 
gpio.setmode(gpio.BOARD) 
# 设置GPIO口为输出 
gpio.setup(in1, gpio.OUT) 
gpio.setup(in2, gpio.OUT) 
gpio.setup(in3, gpio.OUT) 
gpio.setup(in4, gpio.OUT) 
# 设置PWM波,频率为500Hz 
pwm1 = gpio.PWM(in1, 500) 
pwm2 = gpio.PWM(in2, 500) 
pwm3 = gpio.PWM(in3, 500) 
pwm4 = gpio.PWM(in4, 500) 
# 初始化 
pwm1.start(0) 
pwm2.start(0) 
pwm3.start(0) 
pwm4.start(0) 
# 定义向前 
def go(): 
    pwm1.ChangeDutyCycle(50) 
    pwm2.ChangeDutyCycle(0) 
    pwm3.ChangeDutyCycle(50) 
    pwm4.ChangeDutyCycle(0) 
# 定义向右 
def right(): 
    pwm1.ChangeDutyCycle(50) 
    pwm2.ChangeDutyCycle(0) 
    pwm3.ChangeDutyCycle(30) 
    pwm4.ChangeDutyCycle(0) 
# 定义向左 
def left(): 
    pwm1.ChangeDutyCycle(30) 
    pwm2.ChangeDutyCycle(0) 
    pwm3.ChangeDutyCycle(50) 
    pwm4.ChangeDutyCycle(0) 
# 定义向后 
def back(): 
    pwm1.ChangeDutyCycle(0) 
    pwm2.ChangeDutyCycle(50) 
    pwm3.ChangeDutyCycle(0) 
    pwm4.ChangeDutyCycle(50) 
# 定义停止 
def stop(): 
    pwm1.ChangeDutyCycle(0) 
    pwm2.ChangeDutyCycle(0) 
    pwm3.ChangeDutyCycle(0) 
    pwm4.ChangeDutyCycle(0) 
pwm1.stop() 
pwm2.stop() 
pwm3.stop() 
pwm4.stop() 
gpio.cleanup()
```


# 三、视觉巡线

```python
# coding:utf-8 
# 加入摄像头模块，让小车实现自动循迹行驶 
# 思路为：摄像头读取图像，进行二值化，将白色的赛道凸显出来 
# 选择下方的一行像素，黑色为0，白色为255 # 找到白色值的中点 
# 目标中点与标准中点（320）进行比较得出偏移量 
# 根据偏移量来控制小车左右轮的转速 
# 考虑了偏移过多失控->停止;偏移量在一定范围内->高速直行(这样会速度不稳定，已删) 
import RPi.GPIO as gpio 
import time 
import cv2 
import numpy as np 
import serial

ser=serial.Serial('/dev/ttyAMA0',115200,timeout=1)
# 定义引脚 
pin1 = 12 
pin2 = 16 
pin3 = 18 
pin4 = 22 
# 设置GPIO口为BOARD编号规范 
gpio.setmode(gpio.BOARD) 
# 设置GPIO口为输出 
gpio.setup(pin1, gpio.OUT) 
gpio.setup(pin2, gpio.OUT) 
gpio.setup(pin3, gpio.OUT) 
gpio.setup(pin4, gpio.OUT) 
# 设置PWM波,频率为500Hz 
pwm1 = gpio.PWM(pin1, 500) 
pwm2 = gpio.PWM(pin2, 500) 
pwm3 = gpio.PWM(pin3, 500) 
pwm4 = gpio.PWM(pin4, 500) 
# pwm波控制初始化 
pwm1.start(0) 
pwm2.start(0) 
pwm3.start(0) 
pwm4.start(0) 
# center定义 
center = 320 
# 打开摄像头，图像尺寸640*480（长*高），opencv存储值为480*640（行*列） 
cap = cv2.VideoCapture(0) 
while (1): 
    ret, frame = cap.read()
    cv2.imshow("recognize_face", frame) 
    # 转化为灰度图 
    gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY) 
    # 大津法二值化 
    retval, dst = cv2.threshold(gray, 0, 255, cv2.THRESH_OTSU) 
    # 膨胀，白区域变大 
    dst = cv2.dilate(dst, None, iterations=2) 
    cv2.imshow("dst", dst)
    # # 腐蚀，白区域变小 # 
    #dst = cv2.erode(dst, None, iterations=6) 
    # 单看第400行的像素值s 
    color = dst[400] 
    try:
        # 找到白色的像素点个数，如寻黑色，则改为0
        white_count = np.sum(color == 255)
        # 找到白色的像素点索引，如寻黑色，则改为0
        white_index = np.where(color == 255)
        # 防止white_count=0的报错
        if white_count == 0:
            white_count = 1
        # 找到黑色像素的中心点位置
        # 计算方法应该是边缘检测，计算白色边缘的位置和/2，即是白色的中央位置。
        center = (white_index[0][white_count - 1] + white_index[0][0]) / 2
        # 计算出center与标准中心点的偏移量，因为图像大小是640，因此标准中心是320，因此320不能改。
        direction = center - 320
        print(direction)
        ser.write(direction)
    except:
        continue

    # 停止 
    if abs(direction) > 250: 
        pwm1.ChangeDutyCycle(0) 
        pwm2.ChangeDutyCycle(0) 
        pwm3.ChangeDutyCycle(0) 
        pwm4.ChangeDutyCycle(0) 
    # 右转 
    elif direction >= 0: 
        # 限制在70以内 
        if direction > 70: 
            direction = 70 
        pwm1.ChangeDutyCycle(30 + direction) 
        pwm2.ChangeDutyCycle(0) 
        pwm3.ChangeDutyCycle(30) 
        pwm4.ChangeDutyCycle(0) 
    # 左转 
    elif direction < -0: 
        if direction < -70: 
            direction = -70 
        pwm1.ChangeDutyCycle(30) 
        pwm2.ChangeDutyCycle(0) 
        pwm3.ChangeDutyCycle(30 - direction) 
        pwm4.ChangeDutyCycle(0) 
    if cv2.waitKey(1) & 0xFF == ord('q'): 
        break 
# 释放清理 
cap.release() 
cv2.destroyAllWindows() 
pwm1.stop() 
pwm2.stop() 
pwm3.stop() 
pwm4.stop() 
gpio.cleanup() 
```

参考文献：
[树莓派小车自动循迹（摄像头）](https://blog.csdn.net/yzy_1996/article/details/85318179#_15)