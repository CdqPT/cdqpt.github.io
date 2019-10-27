---
title: cmath库函数
author: 陈德强
date: 2019-09-27 22:50:00
categories:
- 字典
tags:
- CPlus
toc: true
top: false
img: /medias/paperimg/c.jpg
summary: cmath函数列表。
---



cmath是c++语言中的库函数，其中的c表示函数是来自c标准库的函数，math为数学常用库函数。

cmath中常用库函数：

|函数|作用|
|:---:|:---:|
|int abs(int i);|返回整型参数i的绝对值|
|double fabs(double x);|返回双精度参数x的绝对值|
|long labs(long n);|返回长整型参数n的绝对值|
|double exp(double x);|返回指数函数e^x的值|
|double log(double x);|返回logex的值|
|double log10(double x)| 返回log10x的值|
|double pow(double x,double y)| 返回x^y的值|
|double pow10(int p) |返回10^p的值|
|double sqrt(double x) |返回+√x的值|
|double acos(double x)| 返回x的反余弦arccos(x)值,x为弧度，1弧度=180/π≈57.3度|
|double asin(double x) |返回x的反正弦arcsin(x)值,x为弧度，1弧度=180/π≈57.3度|
|double atan(double x) |返回x的反正切arctan(x)值,x为弧度，1弧度=180/π≈57.3度|
|double cos(double x) |返回x的余弦cos(x)值,x为弧度，1弧度=180/π≈57.3度|
|double sin(double x) |返回x的正弦sin(x)值,x为弧度，1弧度=180/π≈57.3度|
|double tan(double x) |返回x的正切tan(x)值,x为弧度，1弧度=180/π≈57.3度|
|double hypot(double x,double y) |返回直角三角形斜边的长度(z),x和y为直角边的长度,z^2=x^2+y^2|
|double ceil(double x)| 返回不小于x的最小整数|
|double floor(double x)| 返回不大于x的最大整数|
|int rand()| 产生一个随机数并返回这个数|
|double atof(char *nptr)| 将字符串nptr转换成浮点数并返回这个浮点数|
|double atol(char *nptr) |将字符串nptr转换成长整数并返回这个整数|
|double atof(char *nptr) |将字符串nptr转换成双精度数,并返回这个数,错误返回0|
|int atoi(char *nptr) |将字符串nptr转换成整型数, 并返回这个数,错误返回0|
|long atol(char *nptr)| 将字符串nptr转换成长整型数,并返回这个数,错误返回0|

参考链接：
https://blog.csdn.net/mary19920410/article/details/63696380