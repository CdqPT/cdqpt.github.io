---
title: mrobot功能包编译错误
date: 2019-02-19
categories:
- bug
tags:
- 硕论
img: /medias/paperimg/bug.jpg
toc: true
---

解决mrobot功能包编译错误问题。<!-- more -->

起初我以为是安装包的源码问题，多了一条代码，可是后来发现只要复制粘贴就还是会出现这种问题。一脸懵逼。

解决方案：

找到robot_mrobot/mrobot_navagation/CMakeLists.txt文件，
将第9行的**move_base_msgs**删除后保存，再次编译就能正常通过了。