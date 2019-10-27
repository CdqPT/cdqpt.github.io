---
title: 德飞莱尼莫STM32野火仿真器下载教程
author: 陈德强
date: 2019-04-30 19:39:00
categories:
- 教程
tags:
- mcu
toc: true
top: false
img: /medias/paperimg/mcu.jpg
summary: 德飞莱尼莫STM32野火仿真器下载方教程
---

硬件连接：
STM32-野火仿真器（同时接有秉火SWD TO JTAG）-电脑

软件设置：
options for targets...(锤子)-Debug-CMSIS-DAP Debugger-ok

![image](https://wx3.sinaimg.cn/large/006jQClrly1g2kwvur0btj30v10lmjyz.jpg)
然后点setting，勾选swj，port：SW
![image](https://ws4.sinaimg.cn/large/006jQClrly1g2ky436e7tj30lq0gbtc6.jpg)

如果出现` No Ulink2/Me Device Found`
进行如下修改：
options for targets...(锤子)-Utilities-Use Target Device for Flash Programming-CMSIS-DAP Debugger
![image](https://wx4.sinaimg.cn/large/006jQClrly1g2kyahudxvj30lq0gb0um.jpg)