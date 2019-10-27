
---
title: String类
author: 陈德强
date: 2019-09-27 22:45:00
categories:
- 字典
tags:
- CPlus
toc: true
top: false
img: /medias/paperimg/c.jpg
summary: String类。
---

# 一、初始化
string类有8种初始化方式：


char temp[]="abcdefghigklmn";

|序号|用法|输出|
|:----:|:----:|:----:|
|1|string one("Hello Kitty!");|Hello Kitty!|
|2|string two(3，'$');|$$$|
|3|string three(one);|Hello Kitty!|
|4|string four=two+three;|$$$Hello Kitty!|
|5|string five(five,4);|abcd|
|6|string six(temp+2,temp+6);|cdef|
|7.1|string seven1(&six[1]);|def|
|7.2|string seven2(&six[1],&six[3]);|de|
|8|string eight(six,1,3);|de|

# 二、输入
*C-风格字符串输入*
```c
char info[100];
cin>>info;
cin>>getline(info,100);
cin>>get(info,100);
```
*C++风格字符串输入*
```c
string stuff;
cin>>stuff;
getline(cin,stuff);
```

# 三、方法简介
说明，索引（size_type）即第几个字符。如“qwer”中‘e’的索引为2.
string str="abcdefg";
string st="cd";

|方法|说明|
|:----:|:----:|
|str.size() | 返回字符数|
|str.length()  |返回字符数|
|string::npos|字符串可存储的最大字符数|
|str.find("cd",1)|从str的第2个位置开始查找字符串cd|
|str.find(st,1)|从str的第2个位置开始查找字符串st|
|str.find("cd",1,4)|从str的第2个位置开始到第5个位置查找字符串cd|
|str.find(st,1,4)|从str的第2个位置开始到第5个位置查找字符串st|

# 四、方法详解

## 4.1 字符访问
|命令|功能|
|:---:|:---:|
|string::at| –访问特定字符，带边界检查
|string::operator[]| –访问特定字符
|string::front| –访问第一个字符
|string::back| –访问最后一个字符
|string::data| –访问基础数组，C++11 后与 c_str() 完全相同
|string::c_str| –返回对应于字符串内容的 C 风格零结尾的只读字符串
|string::substr| –以子串构造一个新串；参数为空时取全部源串

## 4.2 迭代器
|命令|功能|
|:---:|:---:|
|string::begin| –获得指向开始位置的迭代器
|string::end| –获得指向末尾的迭代器
|string::rbegin| –获得指向末尾的逆向迭代器
|string::rend| –获得指向开始位置的逆向迭代器
|string::cbegin| –获得指向开始位置的只读迭代器
|string::cend| –获得指向末尾的只读迭代器
|string::crbegin| –获得指向末尾的逆向只读迭代器
|string::crend| –获得指向开始位置的逆向只读迭代器

## 4.3 容量
|命令|功能|
|:---:|:---:|
|string::empty| –检查是否为空
|string::size| –返回数据的字符长度
|string::length| –返回数据的字符长度，与 size() 完全相同
|string::max_size| –返回可存储的最大的字节容量，在 32 位 Windows 上大概为 43 亿字节。
|string::reserve| –改变 string 的字符存储容量，实际获得的存储容量不小于 reserve 的参数值。
|string::capacity| –返回当前的字符存储容量
|string::shrink_to_fit|（C++11 新增）–降低内存容量到刚好

## 4.4 修改器
|命令|功能|
|:---:|:---:|
|string::clear| –清空内容
|string::insert| –插入字符或字符串。如果参数仅为一个迭代器，则在其所指位置插入0 值。
|string::erase| –删除 1 个或 1 段字符
|string::push_back| –追加 1 个字符
|string::pop_back| –删除最后 1 个字符，C++11 标准引入
|string::append| –追加字符或字符串
|string::operator+=| –追加，只有一个参数——字符指针、字符或字符串。
|string::copy| –拷贝出一段字符到 C 风格字符数组；有溢出危险
|string::resize| –改变（增加或减少）字符串长度；如果增加了字符串长度，新字符缺省为 0 值
|string::swap| –与另一个 string 交换内容
|string::replace| –替换子串；如果替换源数据与被替换数据的长度不等，则结果字符串的长度发生改变

## 4.5 搜索
|命令|功能|
|:---:|:---:|
|string::find| –前向搜索特定子串的第一次出现
|string::rfind| –从尾部开始，后向搜索特定子串的第一次出现
|string::find_first_of| –搜索指定字符集合中任意字符在 *this 中的第一次出现
|string::find_last_of| –搜索指定字符集合中任意字符在 *this 中的最后一次出现
|string::find_first_not_of| –*this 中的不属于指定字符集合的首个字符
|string::find_last_not_of| –*this 中的不属于指定字符集合的末个字符
|string::compare| –与参数字符串比较

# 五、栗子
## 5.1 将字符串转化为数字
```c
#include <iostream>
#include <string>

int main()
{
   std::string date = "2019-09-27";
   int stringToNum = atoi((date.substr(0, 4)).c_str());

   std::cout << "the number is:"<<stringToNum << std::endl;

   return 0;
}
```
substr（）是取字符串的部分字符；
c_str()接口是string类的一个函数,返回的是字符串的首地址,返回值类型是`const char *`。如果要使用它并对其进行赋值操作,必须要使用strcpy函数.如果哦直接进行赋值,是不会赋值成功的；
atoi (表示 ascii to integer)是把字符串转换成整型数的一个函数。

# 其他
string类的最大长度为unsigned int的最大值；


参考链接：
https://wikipedia.sogou.se/wiki/String_(C%2B%2B%E6%A0%87%E5%87%86%E5%BA%93)