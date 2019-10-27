---
title: cctype库
date: 2019-09-28 15:51::00
categories:
- 字典
tags:
- CPlus
img: /medias/paperimg/c.jpg
toc: true
summary: cctype库。
---

ctype.h是C标准函数库中的头文件，定义了一批C语言字符分类函数（C character classification functions），用于测试字符是否属于特定的字符类别，如字母字符、控制字符等等。既支持单字节字符，也支持宽字符。

|函数|作用|
|:---:|:---:|
|isalnum		|是否为字母数字|
|isalpha		|是否为字母|
|islower		|是否为小写字母|
|isupper		|是否为大写字母|
|isdigit		|是否为数字|
|isxdigit	|是否为16进制数字|
|iscntrl		|是否为控制字符|
|isgraph		|是否为图形字符（例如，空格、控制字符都不是）|
|isspace		|是否为空格字符（包括制表符、回车符、换行符等）|
|isblank		|是否为空白字符 (C99/C++11新增)（包括水平制表符）|
|isprint		|是否为可打印字符|
|ispunct		|是否为标点|
|tolower		|转换为小写|
|toupper		|转换为大写|