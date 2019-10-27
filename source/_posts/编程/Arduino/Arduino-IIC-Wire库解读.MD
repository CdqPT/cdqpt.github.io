---
title: Arduino-IIC-Wire库解读
date: 2019-02-05
categories:
- 字典
tags:
- mcu
toc: true
img: /medias/paperimg/arduino.jpg
---

Wire.h是Arduino的IIC库。<!-- more -->
# 一、Wire库函数

1.  Wire.begin()
2.  Wire.requestFrom()
3.  Wire.beginTransmission()
4.  Wire.endTransmission()
5.  Wire.write()
6.  Wire.available()
7.  Wire.read()
8.  Wire.onReceive()
9.  Wire.onRequest()

# 二、库函数详细介绍

## 1、Wire.begin() 和 Wire.begin(address)

### 描述 

初始化wire库 ，并且加入到I2C网络，前者作为Master 或 Slaver，并且只能调用一次。

### 参数

address ：7位的器件地址（可选），如果没有输入，则以Master的形式加入到I2C网络。

## 2、Wire.requestFrom()

### 描述

主设备请求从设备一个字节，这个字节可以被主设备用 read()或available()接受。

Wire.requrstFrom(addtess,quantity)

Wire.requrstFrom(addtess,quantity,stop)

### 参数

addtess ： 7位的器件地址

quantity ： 请求得到的数量

stop ：布尔形，‘1’ 则在请求结束后发送一个停止命令，并释放总线。‘0’则继续发送请求保持连接。

### 返回

字节形 ，从从机接受到的字节数目。

## 3  Wire.beginTransmission(address)

### 描述 

开始一次传输数据，发送一个I2C开始字符

### 参数 

address ： 器件的7位地址

### 返回值 无

## 4、 Wire.endTransmission()和Wire.endTransmission(stop)

### 描述

结束一个由beginTransmission（）开始的并且由write（）排列的从机的传输。在Arduino中 endTransmission（）接受到

一个布尔形变量，如果为1 则endTransmission（）发送一个停止信息；如果为0 则发送开始信息

### 返回值

0 成功 1 数据溢出 2 发送addtess时从机接受到NACK 3 发送数据时接受到 NACK 4 其他错误

## 5、 Wire.write()

### 描述

向从机发送数据

### 形式

Wire.write(value) 

Wire.write(string)  

Wire.write(data, length) 

### 参数

value ：要发送的数值

string ：字符组的指针

data ：一个字节数组

length ：传输的数量

### 例子：

```
#include <Wire.h>
 
byte val = 0;
 
void setup()
{
   Wire.begin(); // join i2c bus
}
 
void loop()
{
   Wire.beginTransmission(44); // transmit to device #44 (0x2c)
                               // device address is specified in datasheet
   Wire.write(val);             // sends value byte  
   Wire.endTransmission();     // stop transmitting
 
   val++;        // increment value
   if(val == 64) // if reached 64th position (max)
   {
     val = 0;    // start over from lowest value
   }
   delay(500);
}
```

## 6、  Wire.available()

描述
Wire.requestFrom()请求从机数据后，可以使用available接收

## 7、 Wire.read()

描述
Wire.requestFrom()请求从机数据后，可以使用read接收

## 8、 Wire.onReceive()

描述
从机接收主机发来的数据

## 9、 Wire.onRequest()

描述
从机请求主机发送数据

参考自：https://blog.csdn.net/zhao_120/article/details/47028415
