---
title: python-doesnotmatchanyouter
date: 2019-04-14 20:25:00
categories: bug
tags: python
toc: true
img: /medias/paperimg/bug.jpg
summary: BUG-IndentationErrord--unindent does not match any outer indentation level
author: 陈德强
top: 
password: 
cover: 
---

用Spyder时候出现如下BUG:
IndentationError: unindent does not match any outer indentation level
原因是缩进使用了空格
然而在Spyder里看不出来
把代码复制到sublime中
使用Ctrl+h，替换空格就会显示出被空格代替的缩进，改掉就好了。
