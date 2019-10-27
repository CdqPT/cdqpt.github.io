
---
title: hexoSpawnfailed
date: 2019-09-21 08:14:00
categories: bug
tags: 博客
toc: true
img: /medias/paperimg/bug.jpg
summary: sudo spctl --master-disable
author: 陈德强
top: 
password: 
cover: 
---

hexo d的时候报错
```
Error: Spawn failed
    at ChildProcess.<anonymous> (/Users/cdq/Documents/GitRep10/node_modules/hexo-util/lib/spawn.js:52:19)
```
原因好像是线程阻塞
解决方案：
```
ulimit -n 10000
```
然后在重新hexo g和hexo d就好了


后来由出错了，查到一个办法是换手机热点，
居然成功了
why？？？