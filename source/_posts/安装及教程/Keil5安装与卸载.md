---
title: Keil5安装与卸载
author: 陈德强
date: 2019-04-08 10:32:00
categories:
- 教程
tags:
- mcu
toc: true
top: false
img: /medias/paperimg/mcu.jpg
summary: Keil5安装与卸载
---

# 一、Keil5安装教程
# 1.1 安装
[百度网盘下载地址](https://pan.baidu.com/s/1uk4taNVBExzy0CaXAYGNVA) 提取码: ubfk 

![image](https://wx2.sinaimg.cn/large/006jQClrly1g1v109c4bnj30n306jglr.jpg)

由上到下分别是:1.51编程模块;2.开发板全型号库;3.Keil破解文件;4.Keil5安装包。
先安装4，一路默认，需要填写的随便填就可以，弹窗选同意；
再安装1，一路默认，需要填写的随便填就可以，弹窗选同意；
再安装2，一路默认。
# 1.2 破解

没破解的Keil只能编译小文件，破解后无限制。

上面操作完后，桌面会有Keil5图标，`右键管理员`打开Keil5，选择注册管理；
![image](https://ws4.sinaimg.cn/large/006jQClrly1g1v18fpah3j30yp0e30uq.jpg)

复制CDI号
![image](https://wx2.sinaimg.cn/large/006jQClrly1g1v19z00zxj30lu0gvace.jpg)


`右键管理员`打开Keil破解文件；
粘贴CDI，工程选择C51，点击生成；
![image](https://ws3.sinaimg.cn/large/006jQClrly1g1v1axor6tj30d80gjmyz.jpg)
复制生成码，回到keil注册管理器，粘贴生成码，选中51，点击添加LIC，破解成功后会有succeful提示，如下：
![image](https://ws1.sinaimg.cn/large/006jQClrly1g1v1djk380j30lu0gv0vk.jpg)
此时51破解完成；
再回到破解软件，将工程中的C51换成ARM，再次点击生成，
![image](https://wx1.sinaimg.cn/large/006jQClrly1g1v1gz83oaj30d80gj769.jpg)
复制生成码，回到keil注册管理器，粘贴生成码，选中ARM，点击添加LIC，破解成功后会有succeful提示。
此时ARM破解完成。

# 二、Keil5卸载教程

假如之前安装了Keil4，现在开始卸载：

使用控制面板或者360软件管家卸载都可以，重要的是后边；

卸载完后删除C盘根目录存留的Keil文件夹；

然后`WIN+r`打开运行，输入`regedit`打开注册表，再进入HKEY_CLASSER_ROOT选项，
下拉滑动条找到UVPROJFILE（KEIL工程文件类型）
将这几个UV开头的都删掉
![image](https://wx2.sinaimg.cn/large/006jQClrly1g1v1qwuhhaj30wl0d2whd.jpg)
重启电脑后就卸载完成了。