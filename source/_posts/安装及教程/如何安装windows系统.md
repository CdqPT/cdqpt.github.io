---
title: 如何安装windows系统
date: 2019-1-9
categories:
- 教程
tags:
- windows
toc: true
img: /medias/paperimg/other.jpg

---
装系统有两种方式，一种是下载系统镜像文件后解压ios文件到除c盘以外其他盘都可（如原系统是win10系统，则可以直接右键加载，而不必解压），然后运行.exe文件就可以自动安装了。这种方法在新款电脑上不适用，就要用到第二种方法；第二种是使用u盘作为启动盘制作系统。下面介绍一下第二种安装方式，以Windows10为例。<!-- more -->
安装前需要准备：

8G以上u盘一个。
# 一、制作u盘启动盘

插上u盘；

百度搜索“老毛桃u盘制作工具”，官网下载“装机版下载”，下载并安装；

![image](http://upload-images.jianshu.io/upload_images/16115686-1d3b11629e4f3d78.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

打开老毛桃，选择要制作的u盘，其余选项全都默认，点击一键制作。

制作完毕后会有弹窗，点击否就可以。 

# 二、下载系统镜像文件

以win10系统为例，可以百度一下“win10下载”，

如系统之家，雨林木风等等，或者胡萝卜周：[http://www.carrotchou.blog/9653.html](http://www.carrotchou.blog/9653.html)

将下载好的镜像文件放在除了C盘以外任何位置都行，如D盘，当然大多数人会选择放在U盘启动盘里。

# 三、设置BIOS第一启动项

每台电脑的BIOS设置都不一样，有的改起来有些麻烦，有的不用更改。

如果这一步不会设置，可以跳过，直接到第三步，有一定几率会成功。

开机按F2（不同电脑对应的按键不同，可百度查询，如百度一下联想电脑进入BIOS按键）进入BIOS选项；

在BOOT选项中将u盘启动项设置为第一启动项（每个电脑主板显示不同，可百度一下自己电脑型号的u盘第一启动项设置方式），这样F10保存重启后就会自动进入PE环境；

注意：”如果重启后出现“Invalid Partition Table.....”，就是启动项不对，需要在BIOS切换成UEFI或Legency，如果已经加了固态硬盘，可能还需要调整硬盘启动顺序，挨个试吧，总有一个是对的。

# 四、进入启动盘选项

### 如果设置了u盘作为第一启动项，则这步可以选择跳过，也可以选择操作。

如果不会设置u盘作为第一启动项，则按F8（不同电脑对应的按键不同，可百度查询，如百度联想电脑u盘启动项按键），然后会弹出如下界面，选择对应的u盘；

如果不知道哪个是自己u盘，可以拍下来列表，拔出u盘，再ctrl+alt+delete重启后按F8进入启动列表，哪个消失了哪个就是u盘；

如果发现没有u盘列表，那么就必须要进入BIOS将U盘设置为第一启动项才可以识别。

![image](http://upload-images.jianshu.io/upload_images/16115686-e396b0c2d566c4be.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 进入有如下界面： 

![image](http://upload-images.jianshu.io/upload_images/16115686-8832d27540830c2c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 然后进入如下界面：

![image](http://upload-images.jianshu.io/upload_images/16115686-0de6716895ae6db2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

###  然后等着就行了：  

![image](http://upload-images.jianshu.io/upload_images/16115686-88dfcdfe1154a868.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
