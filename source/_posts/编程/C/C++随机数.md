---
title: C++随机数
author: 陈德强
date: 2019-10-05 15:50:00
categories:
- 字典
tags:
- CPlus
toc: true
top: false
img: /medias/paperimg/c.jpg
summary: C++随机数。
---



C++通过一组协作的类来产生随机数。随机数引擎类可以生成unsigned随机数序列，随机数分布类可以生成服从特定概率分布的随机数。

```c
#include <iostream>
#include<random>

using std::cout;
using std::endl;
int main()
{
   std::default_random_engine e;//定义随机数引擎
   std::uniform_int_distribution<unsigned> id(1, 10);//整型分布
   std::uniform_real_distribution<double> dd(0, 1.0);//浮点型分布

   e.seed(10);//设置随机数种子
   for (size_t i = 0; i < 10; i++)
   {
      cout << id(e) << " ; " << dd(e) << endl;
   }
}
```