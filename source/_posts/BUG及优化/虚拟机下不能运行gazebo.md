---
title: 虚拟机下不能运行gazebo
date: 2019-1-29
categories:
- bug
tags:
- 硕论
toc: true
img: /medias/paperimg/bug.jpg
---
bug描述：

VMware: vmw_ioctl_command error Invalid argument<!-- more -->
解决方式：设置环境变量
```
export SVGA_VGPU10=0
```
或者
```
echo "export SVGA_VGPU10=0" >> ~/.bashrc
```
