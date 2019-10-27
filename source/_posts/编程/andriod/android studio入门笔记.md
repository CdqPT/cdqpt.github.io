---
title: android studio入门笔记
date: 2019-05-08 08:00:00
categories: 笔记
tags: 安卓
toc: true
img: /medias/paperimg/android.jpg
summary: android studio入门笔记。
author: 陈德强
top: 
password: 
cover: 
---
# 一、创建工程
创建工程详见官网教程：
[https://developer.android.google.cn/training/basics/firstapp/creating-project]（https://developer.android.google.cn/training/basics/firstapp/creating-project）
这里需要注意一下，第一次创建时候会加载一段时间，普通的电脑可能加载好一会，加载后的输出栏会全打上对号，并且工程目录上显示的只有app和gradle scripts两个项目。
# 二、运行app
运行app时点击右上角的三角号，这时会有两个选择。
第一，在`手机`上显示运行的程序。
这种方式直接连接安卓手机就会显示。
第二，在`安卓模拟器`上显示运行程序。
这种方式在点击三角号后，界面下边会有一个选项，creat new virtual device
这时就显示创建那种虚拟机，Pixel系列就可以，
这时候重点来了，
由于使用的是外网下载，随时有可能被墙，
网好的时候才能下载模拟器，网不好的时候会提示你没连接到网络，
这时你可以选择过两天再下载，或者手动下载，我使用了第一种方式。。。
# 三、界面布局
## 3.1 添加一个文本框
Palette->Text
## 3.2 添加一个按钮
Palette->Buttons
## 3.3 更改界面字符串
Project -> app -> res -> values -> strings.xml



参考文献：[Android Developers Docs 指南](https://developer.android.google.cn/training/basics)