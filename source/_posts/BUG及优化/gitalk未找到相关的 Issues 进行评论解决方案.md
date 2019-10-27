---
title: gitalk未找到相关的 Issues 进行评论解决方案
date: 2019-02-12
categories:
- bug
tags:
- 博客
img: /medias/paperimg/bug.jpg
toc: true
---
原因是Authorization callback URL没有设置对，一定要是https才能用<!--more-->

 进入当前库的setting，在右上角


进入developer settings


 进入OAuth Apps，如果没有请创建


 将底部的Authorization callback URL修改为https://,而不是http:// 



点击update就可以了


 此时再刷新界面点击登陆就可以了