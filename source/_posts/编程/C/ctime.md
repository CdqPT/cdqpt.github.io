---
title: ctime
author: 陈德强
date: 2019-09-27 22:12:00
categories:
- 字典
tags:
- CPlus
toc: true
top: false
img: /medias/paperimg/c.jpg
summary: ctime。
---

**日历时间（Calendar Time）**，是从一个标准时间点（epoch）到现在的时间经过的秒数，不包括插入闰秒对时间的调整。开始计时的标准时间点，各种编译器一般使用UTC 1970-01-01 00:00:00。日历时间用数据类型time_t表示。[1]:20time_t类型实际上一般是32位整数类型，因此表示的时间不能晚于UTC 2038-01-18 19:14:07。为此，某些编译器引入了64位甚至更长的整型来保存日历时间，如Visual C++支持__time64_t数据类型，通过_time64()函数获取日历时间，可支持到UTC 3001-01-01 00:00:00的时间。

**分解时间(broken-down time)**，用结构数据类型tm表示。

# 一、tm数据结构
```c
struct tm {
　　int tm_sec; /* 秒 – 取值区间为[0,59] */
　　int tm_min; /* 分 - 取值区间为[0,59] */
　　int tm_hour; /* 时 - 取值区间为[0,23] */
　　int tm_mday; /* 日期 - 取值区间为[1,31] */
　　int tm_mon; /* 月份 - 取值区间为[0,11] (0代表一月)*/
　　int tm_year; /* 年份 - 其值等于实际年份减去1900 */
　　int tm_wday; /* 星期 – 取值区间为[0,6]，其中0代表星期天，1代表星期一，以此类推 */
　　int tm_yday; /* 天数 – 取值区间为[0,365]，其中0代表1月1日，1代表1月2日 */
　　int tm_isdst; /* 夏令时标识符,实行时tm_isdst为正,不实行时为0；不了解情况时为负。*/
　　};
```

# 二、函数

## 2.1 获取当前时间
`time_t time(time_t* timer)`
得到从标准计时点（一般是1970年1月1日午夜）到当前时间的秒数。
`clock_t clock(void)`
得到从进程启动到此次函数调用的累计的时钟滴答数。
## 2.2 日期转换
`struct tm* gmtime(const time_t* timer) `
从日历时间time_t到分解时间tm（世界协调时**UTC**）的转换。
`struct tm* localtime(const time_t* timer)`
从日历时间time_t到分解时间tm的转换，即结果数据已经调整到**本地时区**与夏令时。
`time_t mktime(struct tm* ptm)`
从基于本地时区(与夏令时)的分解时间tm到日历时间time_t的转换。
---------
`time_t timegm(struct tm* brokentime)`
从分解时间tm（被视作UTC时间，不考虑本地时区设置）到日历时间time_t的转换。该函数较少被使用。
`struct tm* gmtime_r(const time_t* timer, struct tm* result)`
该函数是gmtime函数的线程安全版本。

## 2.3 日期格式化
`char *asctime(const struct tm* tmptr)`
把分解时间tm输出到字符串，结果的格式为"Www Mmm dd hh:mm:ss yyyy"，即“周几 月份数 日数 小时数:分钟数:秒钟数 年份数”。函数返回的字符串为静态分配，长度不大于26，与ctime函数共享。函数的每次调用将覆盖该字符串内容。
`char* ctime(const time_t* timer)`
把日历时间time_t timer输出到字符串，输出格式与asctime函数一样.
`size_t strftime(char* s, size_t n, const char* format, const struct tm* tptr)`
把分解时间tm转换为自定义格式的字符串，类似于常见的字符串格式输出函数sprintf。
`char * strptime(const char* buf, const char* format, struct tm* tptr)`
strftime的逆操作，把字符串按照自定义的格式转换为分解时间tm。

## 2.4 日期差
`double difftime(time_t timer2, time_t timer1)`
比较两个日历时间之差。

# 三、栗子

## 3.1 输出当前时间

```c
#include <stdio.h>
#include <string.h>
#include <time.h>
 
int main(void)
{
    struct tm t;   //tm结构指针
	time_t now;  //声明time_t类型变量
	time(&now);      //获取系统日期和时间
	localtime_s(&t, &now);   //获取当地日期和时间
        
    //格式化输出本地时间
    printf("年：%d\n", t.tm_year + 1900);
	printf("月：%d\n", t.tm_mon + 1);
	printf("日：%d\n", t.tm_mday);
	printf("周：%d\n", t.tm_wday);
	printf("一年中：%d\n", t.tm_yday);
	printf("时：%d\n", t.tm_hour);
	printf("分：%d\n", t.tm_min);
	printf("秒：%d\n", t.tm_sec);
	printf("夏令时：%d\n", t.tm_isdst);
	system("pause");

	return 0;
```
```
年：2019
月：9
日：3
周：2
一年中：245
时：10
分：49
秒：1
夏令时：0
Press any key to continue . . .
```

## 3.2 获取目标时间到今天的天数
```c
#include <iostream>
#include <ctime>

int main()
{
   time_t nowTimeSec;  
   time(&nowTimeSec);      

   struct std::tm originTime = { 0,0,0,1,0,76 };//1976.01.01
   std::time_t originTimeSec = std::mktime(&originTime);
   int difference = std::difftime(nowTimeSec, originTimeSec) / (60 * 60 * 24);
   std::cout << "MapAge = " << difference << " days" << std::endl;

   return 0;
}
```
```
MapAge = 15944 days
```
## 3.3 获取任意两天的天数差
```c
#include <iostream>
#include <ctime>

int main()
{
    struct std::tm a = {0,0,0,24,5,104}; /* June 24, 2004 */
    struct std::tm b = {0,0,0,5,6,104}; /* July 5, 2004 */
    std::time_t x = std::mktime(&a);
    std::time_t y = std::mktime(&b);
    if ( x != (std::time_t)(-1) && y != (std::time_t)(-1) )
    {
        double difference = std::difftime(y, x) / (60 * 60 * 24);
        std::cout << "difference = " << difference << " days" << std::endl;
    }
    return 0;
}
```
```
difference = 11 days
```


参考链接：
https://cloud.tencent.com/developer/ask/49142
https://wikipedia.sogou.se/wiki/Time.h