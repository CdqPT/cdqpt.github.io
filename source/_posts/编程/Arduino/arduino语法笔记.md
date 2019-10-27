---
title: arduino语法笔记
date: 2019-05-08 20:41:00
categories: 笔记
tags: mcu
toc: true
img: /medias/paperimg/arduino.jpg
summary: arduino语法笔记。
author: 陈德强
top: 
password: 
cover: 
---

# 一、数字I/O口

## 1.1 初始化引脚
```
pinMode(1, OUTPUT);
pinMode(1, INPUT);
pinMode(1, INPUT_PULLUP);//上拉输入
```
## 1.2 输出电平
```
digitalWrite(13,HIGH);
digitalWrite(13,LOW);
```
## 1.3 读取电平
```
digitalRead(HIGH)
digitalRead(LOW)
```
# 二、模拟I/O口
模拟I/O口不需要初始化
```
analogWrite(pin,value);//引脚值，占空比
analogRead(pin);//引脚值
```

# 三、延时
单位为ms
```
delay(500);
```

# 四、串口
## 4.1 初始化串口
```
Serial.begin(9600);
```
## 4.2 查询数据长度
```
Serial.available()
```
## 4.3 读取串口数据
```
Serial.read()
```
## 4.4 串口输出字符串
```
Serial.print("on");
或可选格式
Serial.print(78, BIN);//以二进制输出
```
## 4.5 串口输出字符串并换行
```
Serial.println("on");
```

# END 其他函数
将数据转化为字符串：
```
char(XXX)
```












