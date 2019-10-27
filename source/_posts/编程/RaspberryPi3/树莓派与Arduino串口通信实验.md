---
title: 树莓派与Arduino串口通信实验
date: 2019-04-20 9:35:00
categories: 笔记
tags: 嵌入式
toc: true
img: /medias/paperimg/raspi.jpg
summary: 树莓派与Arduino串口通信实验。
author: 陈德强
top: 
password: 
cover: 
---

目标：树莓派通过串口发送字符's'，Arduino收到后字符's'后打印字符串'I AM CDQ',同时arduino自带的13引脚LED灯会闪烁。

这篇写的有点乱，有不清楚地方请向我反映，我会及时修改。

# 一、Arduino程序
在arduinoIDE中编写程序
```c
void setup()
{
  Serial.begin(115200);
  pinMode(13,OUTPUT);
  }
void loop()
{
  if(Serial.available())
  {
    if('s'==Serial.read())
    Serial.println("I AM CDQ");
    digitalWrite(13,HIGH);
    delay(500);
    digitalWrite(13,LOW);
    delay(500);
    }
  
  }
```
将程序烧写到Arduino中，
然后将USB口从电脑上拔掉，插到树莓派USB上，否则串口会被占用。

# 二、开启树莓派串口

树莓派有两个串口，一个是mini，一个是AMA0，mini是自带晶振驱动的，稳定性不高，AMA0是外部晶振驱动的，稳定性高，因此这里将启用AMA0串口。

使用nano编辑器打开启动命令文本：
```
sudo nano /boot/cmdline.txt
```
将内容修改为：
`ddwc_otg.lpm_enable=0 console=tty1 root=/dev/mmcblk0p2 rootfstype=ext4 elevator=vator=deadline  rootwait quiet splash plymouth.ignore-serial-consoles`
Ctrl+x
y
回车
输入：
```
sudo nano /etc/inittab
```
注释最后一行，如果没有则不用修改：
`#T0:23:respawn:/sbin/getty -L ttyAMA0 115200 vt100`

查看串口是否启用
```
ls -l /dev
```
![image](https://ws4.sinaimg.cn/large/006jQClrly1g28vwqjfc2j30mj0eftis.jpg)
如果没成功，则参考【文献1】再设置一遍。

# 三、下载串口调试助手minicom

不要指望他会有什么好看的画面，哈哈哈。
安装minicom
```
sudo apt-get install minicom
```
启动minicom
```
minicom -b 115200 -o -D /dev/ttyAMA0
```

发送测试信息，如abc：
如果回显开启，则你输入一个字母他也同样输出相同的字母，效果为aabbcc，
如果此时没有出现aabbcc，则可能是回显没有打开，Ctrl+a，再按e，打开回显，再次测试。

或者连接好树莓派与arduino的串口后输入s，看minicom是否有返回`I AM CDQ`

树莓派与Arduino的接线方式为：

|树莓派|arduino|
|:-------:|:-------:|
|8脚（TXD）|0脚（RX）|
|10脚（RXD）|1脚（TX）|
|usb口|下载线|


# 四、树莓派程序

在树莓派桌面新建ser.py文件
```python
import serial
ser=serial.Serial('/dev/ttyAMA0',115200,timeout=1)

try:
    while 1:
        ser.write('s')
        response=ser.readall()
        print (response)
except:
    ser.close()
```

不要使用Thonny软件运行此程序，因为看不到输出。

```
/home/pi/Desktop
```
```
sudo python ser.py
```
此时应该就会看到：
![image](https://wx4.sinaimg.cn/large/006jQClrly1g28wq9x9vqj30mm0f0abw.jpg)

同时，arduino的灯会闪烁。



参考文献：
1. [树莓派3b与电脑串口互相通信进行数据传输的配置过程](https://blog.csdn.net/qq_36326623/article/details/79780061)
2. [树莓派开发笔记(六)：GPIO口的UART使用](https://blog.csdn.net/qq21497936/article/details/79758975)
3. [树莓派与arduino串行通信](https://blog.csdn.net/huayucong/article/details/47068953)
4. [两大开源硬件之树莓派与Arduino的USB串口通讯](https://blog.csdn.net/qq_18402617/article/details/81414541)
