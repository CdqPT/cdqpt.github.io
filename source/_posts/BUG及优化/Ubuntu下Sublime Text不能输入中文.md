---
title: Ubuntu下Sublime Text不能输入中文
date: 2019-04-14 15:49:00
categories: bug
tags: linux
toc: true
img: /medias/paperimg/bug.jpg
summary: Ubuntu下Sublime Text不能输入中文解决方案
author: 陈德强
top: 
password: 
cover: 
---


```
sudo apt-get update && sudo apt-get upgrade
```
```
git clone https://github.com/lyfeyaj/sublime-text-imfix.git
```
```
cd sublime-text-imfix
```
```
./sublime-imfix
```
OK!


参考地址：[https://github.com/lyfeyaj/sublime-text-imfix](https://github.com/lyfeyaj/sublime-text-imfix)