---
title: linux设置开机自启
date: 2019-10-15 23:09:00
categories: 教程
tags: linux
toc: true
img: /medias/paperimg/linux.jpg
summary: linux设置开机自启
author: 陈德强
top: 
password: 
cover: 

---


# 一、程序自启

这是一个最简单的方法，编辑“/etc/rc.local”，把启动程序的shell命令输入进去即可（要输入命令的全路径），可以通过`which 程序名`命令获得。

使用命令 `vi  /etc/rc.local`   

然后在文件最后一行添加要执行程序的全路径。

例如，每次开机时要执行一个haha.sh，这个脚本放在/opt下面，那就可以在“/etc/rc.local”中加一行“/opt/./haha.sh”，或者两行“cd /opt”和“./haha.sh”。


--------------

在/etc/rc.local中加入程序启动语句 ----- 开机自启动

在~/.bash_profile中加入程序启动语句  ---- 登陆自启动

在~/.bashrc 中加入程序启动语句 ---- 打开终端时自启动


# 二、文件自启

TODO：）