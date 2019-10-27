---
title: tuple库
author: 陈德强
date: 2019-10-05 15:48:00
categories:
- 笔记
tags:
- CPlus
toc: true
top: false
img: /medias/paperimg/c.jpg
summary: tuple库。
---


tuple可以存储不同类型的元素，同时可以用于返回多个值而不必构建结构体。
```c
#include<tuple>
```
# 一、函数
`make_tuple`：函数模板，创建一个tuple对象
`std::get<size_t index>(std::tuple)`或者`std::get<typename>(std::tuple)`：函数模板，访问tuple的特定元素
-----------------------------------------------------
tie：函数模板，创建一个完全由左值引用作为实参而构造出的tuple，其效果相当于解开（unpack）一个tuple的当前值赋给一组单个的对象；其中不关心的元素用std::ignore对应。例如：std::tuple<int,std::ignore,string> t(1,2,"hello");
forward_as_tuple：函数模板，创建一个引用到实参的tuple。如果实参为右值，tuple的数据成员是右值引用；否则为左值引用。
tuple_cat：函数模板，通过连接任意数量的tuple来创建一个tuple
operator== ：函数模板，
operator!= ：函数模板，
operator< ：函数模板，
operator<= ：函数模板，
operator> ：函数模板，
operator>= ：函数模板，
std::swap(std::tuple)：函数模板，std::swap算法的特化
apply：函数模板，C++17引入。调用一个函数，使用tuple作为实参
make_from_tuple：函数模板，C++17引入。构造一个对象，使用tuple作为实参
# 二、获取tuple元素个数
```c
std::tuple<float, string> tup1(3.14, "pi");  
cout << tuple_size<decltype(tup1)>::value;  
```
# 三、获取tuple元素类型
```c
std::tuple<float, string> tup1(3.14, "pi");  
std::tuple_element<0, decltype(tup1)>::type  
```
# 四、栗子
```c
#include <iostream>
#include <tuple>
#include <functional>

int main()
{
    auto t1 = std::make_tuple(10, "Test", 3.14);
    std::cout << "The value of t1 is "
              << "(" << std::get<0>(t1) << ", " << std::get<1>(t1)
              << ", " << std::get<2>(t1) << ")\n";

    int n = 1;
    auto t2 = std::make_tuple(std::ref(n), n);
    n = 7;
    std::cout << "The value of t2 is "
              << "(" << std::get<0>(t2) << ", " << std::get<1>(t2) << ")\n";
}
```
```
The value of t1 is (10, Test, 3.14)
The value of t2 is (7, 1)
```

参考链接：
[https://zh.wikibooks.org/wiki/C%2B%2B/Tuple](https://zh.wikibooks.org/wiki/C%2B%2B/Tuple)
[https://blog.csdn.net/datase/article/details/80156205](https://blog.csdn.net/datase/article/details/80156205)
