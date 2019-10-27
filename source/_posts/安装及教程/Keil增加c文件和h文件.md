---
title: Keil增加c文件和h文件
author: 陈德强
date: 2019-04-30 21:41:00
categories:
- 笔记
tags:
- mcu
toc: true
top: false
img: /medias/paperimg/mcu.jpg
summary: Keil增加c文件和h文件
---

# 一、新建文件

新建文件有两种方式，
一是点击左上角的新建文件，保存的时候用XXX.c命名即可；
二是直接在需要的地方新建.c文件即可。
.h文件同理。

# 二、导入文件

## 2.1 导入c文件
点击Manage Project Items（品字）-Project Items，
Project Targets可以修改项目名称，
Groups可增加项目下的文件夹（不必和实际工程的文件夹对应，但建议写成一样），
Files可增加c文件。

## 2.2 导入h文件
点击`options for targets...(锤子)`-`C/C++`-`Include Paths`，
点击右边的...增加h文件的文件夹即可，
因此建议把所有头文件放到同一个文件夹下，这样就不用每次都添加了！

