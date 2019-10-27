
---
title: C++最大值
date: 2019-09-21 08:23::00
categories:
- 字典
tags:
- CPlus
img: /medias/paperimg/c.jpg
toc: true
summary: C++最大值的定义。
---


C++中, 经常会使用, 某些类型的最大值, 如int的最大整数(INT_MAX), C的函数中, 包含了这些宏定义.

头文件: 
```c
#include <climits>
```
具体参见:

|name	           | expresses	|value*|
|:----:|:---:|:----:|
|CHAR_BIT	   | Number of bits in a char object (byte)	|8 or greater*|
|SCHAR_MIN	   | Minimum value for an object of type signed char	|-127 (-27+1) or less*|
|SCHAR_MAX	   |  Maximum value for an object of type signed char	|127 (27-1) or greater*|
|UCHAR_MAX	|Maximum value for an object of type unsigned char|	255 (28-1) or greater*|
|CHAR_MIN	     |Minimum value for an object of type char	|either SCHAR_MIN or 0|
|CHAR_MAX	     |Maximum value for an object of type char	|either SCHAR_MAX or UCHAR_MAX|
|MB_LEN_MAX	|Maximum number of bytes in a multibyte character, for any locale|	1 or greater*|
|SHRT_MIN	    |Minimum value for an object of type short int	|-32767 (-215+1) or less*|
|SHRT_MAX	    |Maximum value for an object of type short int	|32767 (215-1) or greater*|
|USHRT_MAX   	|Maximum value for an object of type unsigned short int|	65535 (216-1) or greater*|
|INT_MIN	Minimum |value for an object of type int	|-32767 (-215+1) or less*|
|INT_MAX	   |Maximum value for an object of type int	|32767 (215-1) or greater*|
|UINT_MAX	   |Maximum value for an object of type unsigned int	|65535 (216-1) or greater*|
|LONG_MIN	   |Minimum value for an object of type long int	|-2147483647 (-231+1) or less*|
|LONG_MAX	   |Maximum value for an object of type long int	|2147483647 (231-1) or greater*|
|ULONG_MAX	|Maximum value for an object of type unsigned long int	|4294967295 (232-1) or greater*|
|LLONG_MIN	   |Minimum value for an object of type long long int	|-9223372036854775807 (-263+1) or less*|
|LLONG_MAX	   |Maximum value for an object of type long long int	|9223372036854775807 (263-1) or greater*|
|ULLONG_MAX	|Maximum value for an object of type unsigned long long int	|18446744073709551615 (264-1) or greater*|


参考链接：https://blog.csdn.net/caroline_wendy/article/details/24311895