---
title: 关于if()中判断表达式的问题
author: 陈德强
date: 2019-10-24 18:44:00
categories:
- 笔记
tags: CPlus
toc: true
top: 
img: /medias/paperimg/c.jpg
summary: 表达式内进行了运算。
---

if判断的实质是先进行了运算，然后将bool结果返回，
就是说，if判断了的运算执行了。

```c
#include <iostream>

bool test(int& a, int& b);

int main(void)
{
   int m_a(0), m_b(0);
   if (test(m_a, m_b))
      std::cout << m_a<< std::endl << m_b<< std::endl;
   system("pause");
   return 0;
}

bool test(int& a, int& b)
{
   a++;
   b++;
   b++;
   return true;
}
```
```
1
2
```

