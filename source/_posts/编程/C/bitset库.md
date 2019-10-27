---
title: bitset库
author: 陈德强
date: 2019-10-05 15:49:00
categories:
- 字典
tags:
- CPlus
toc: true
top: false
img: /medias/paperimg/c.jpg
summary: bitset库。
---



C++语言的一个类库，用来方便地管理一系列的bit位而不用程序员自己来写代码。
bitset除了可以访问指定下标的bit位以外，还可以把它们作为一个整数来进行某些统计。

# 一、函数
|命令|功能|
|:---:|:---:|
|(constructor)| 构造函数
|all| 测试所有的标志位是否置位
|any| 测试是否有标志位置位
|count| 返回标志位的个数
|flip |翻转所有的或者指定位置的标志位
|none |测试是否没有标志位置位
|reset| 复位所有的标志位
|set |置位所有的标志位
|size| 返回标志位的个数
|test| 测试指定位置的标志位是否置位
|to_string |转化为string表示
|to_ullong |转化为unsigned long long.
|to_ulong |转化为unsigned long


---------------------
operator!=
operator&= 按位与赋值
operator<< 向左移位
operator<<=
operator==
operator>>
operator>>=
operator[] 访问指定的标志位
operator^= 按位异或赋值
operator|= 按位或赋值
operator~ 按位非
# 二、构造函数
```c
bitset<4> bitset1;　　//无参构造，长度为４，默认每一位为０
bitset<8> bitset2(12);　　//长度为８，二进制保存，前面用０补充
string s = "100101";
bitset<10> bitset3(s);　　//长度为10，前面用０补充
char s2[] = "10101";
bitset<13> bitset4(s2);　　//长度为13，前面用０补充
cout << bitset1 << endl;　　//0000
cout << bitset2 << endl;　　//00001100
cout << bitset3 << endl;　　//0000100101
cout << bitset4 << endl;　　//0000000010101
```
参考链接：
[https://zh.wikibooks.org/wiki/C%2B%2B/Bitset](https://zh.wikibooks.org/wiki/C%2B%2B/Bitset)
https://www.baidu.com/link?url=V3XjRzifgSVzhRmUVntzCkojPjSz_zdedSb0GrWjpPG7sBioMcrp1Jykt4IQ3_n0LPUCkRKjaoyN5E-BTtDQSq&wd=&eqid=858f10fa001d6bae000000025d9442e5
