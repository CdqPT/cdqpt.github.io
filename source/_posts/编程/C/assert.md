---
title: assert
author: 陈德强
date: 2019-10-23 22:54:00
categories:
- 笔记
tags:
- CPlus
toc: true
top: false
img: /medias/paperimg/c.jpg
summary: 断言，用于调试。
---

assert断言，用于测试。当判断条件不满足，会终止程序并弹窗，然后会定位到断点处，用以调试。
调试结束后可以用`#define NDEBUG`取消断言。

```c
#include <stdlib.h>
#include <assert.h>
#include <stdio.h>
#pragma warning(disable:4996)  //取消 4996警告！

int main(void)
{
   FILE *fp;

   //以可写的方式打开一个文件，如果不存在则创建它
   fp = fopen("C:\\Users\\Administrator\\Desktop\\test1.txt", "w");
   assert(fp);                           //这里不会出错
   fclose(fp);

   //以只读的方式打开一个文件，如果不存在则打开失败
   fp = fopen("C:\\Users\\Administrator\\Desktop\\test2.txt", "r");
   assert(fp);                           //所以这里出错
   fclose(fp);                           //程序永远都执行不到这里来
   
   system("pause");
   return 0;
}
```