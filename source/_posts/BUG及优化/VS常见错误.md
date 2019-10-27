---
title: VS常见错误
date: 2019-09-21 08:23::00
categories:
- bug
tags:
img: /medias/paperimg/bug.jpg
toc: true
summary: VS常见错误。
---


vs编译时出现错误：


# 一、非静态成员引用必须与特定对象相对

原因是没有给类定义对象，而直接引用了方法，因此要区分引用的是类还是作用域，
如果是类就要先定义对象，再引用方法。


# 二、没有生成“object”文件
严重性	代码	说明	项目	文件	行	禁止显示状态
错误	C2220	警告被视为错误 - 没有生成“object”文件	mgr_mpp	E:\GitRepo\EHP-Navi-dev\mgr\mpp\Streamer\src\adasis\AdasisMessages.cpp	2315	

原因是：定义变量但是没有初始化造成的。


# 三、无法解析的外部符号
解决：头文件没有引用正确，或者头文件没有引用。


# 四、无法填充你的账户
每次打开VS都报错：我们无法自动填充你的 Visual Studio Team Services 帐户
可能是visual studio需要重新验证你的账号，首先请退出visual studio的登陆，重启visual studio后重新登陆，这个报错应该就会消失。

参考链接：http://www.datazx.cn/visualstudioxiangguantaolun/20170910494.html