---
title: python学习笔记
date: 2019-04-06 09:39:00
categories: 笔记
tags: python
toc: true
img: /medias/paperimg/python.jpg
summary: python学习笔记
author: 陈德强
top: 
password: 
cover: 
---
# 0 安装python
去Anaconda的[下载地址](https://www.anaconda.com/distribution/)下载对应python版本的Anaconda安装文件。
#  1 高阶函数
**高阶函数：**高阶函数可以将函数作为输入值或者返回值。
**闭包：**当函数作为返回值时，可以不立即进行运算或赋值。
例如：
```python
def cal_sum(args):
    def sum():
        a=0
        for i in range(1,args):
            a+=i
        return a
    return sum
print(cal_sum(3))
```
此时返回的是函数本身：
```
runfile('C:/Users/Administrator/.spyder-py3/temp.py', wdir='C:/Users/Administrator/.spyder-py3')
<function cal_sum.<locals>.sum at 0x0000021B561E8F28>
```
只有调用该函数时才会显示计算结果，换言之，在调用之前不会进行赋值运算：
```python
def cal_sum(args):
    def sum():
        a=0
        for i in range(1,args):
            a+=i
        return a
    return sum
f=cal_sum(3)
print(f())
```
```
runfile('C:/Users/Administrator/.spyder-py3/temp.py', wdir='C:/Users/Administrator/.spyder-py3')
3
```
# 2 数组
生成一个0-9的数组：
```python
L=list(range(10))
```
其中range是生成0-9元素，list是将元素转化为数组。


数组取值：
```python
L[start,stop,step]
```
# 3 Lambda表达式（匿名函数）
Lambda函数又称匿名函数，匿名函数就是没有名字的函数。
**用法：**
lambda 参数:返回值
**例如：**
```python
lambda x, y : x+y
```
上述程序就是对x和y求和。
# 4 map()函数
map(function, iterable, ...)会根据提供的函数对指定序列做映射,第一个参数是函数，第二个参数是迭代器(数组？)，例如：
```python
f=list(map(abs,[-1,2,4,-5]))
print(f)
```
以上是对数组求绝对值，输出结果如下：
```
runfile('C:/Users/Administrator/.spyder-py3/temp.py', wdir='C:/Users/Administrator/.spyder-py3')
[1, 2, 4, 5]
```
# 4 面向过程和面向对象
面向过程就是分析步骤，面向对象就是分析对象。
首先解释为什么会出现这两种编程思路？
最先有的是面向过程编程，这是人的自发性编程思路，但是编程后期发现当程序量大时面向过程编程很复杂，逻辑很乱。
我举个栗子：
假如现在在上课，老师让小明做加法运算，让小红做减法运算。
面向过程编程思路：定义小明做加法运算的函数，定义小红做减法运算的函数，老师发出命令，执行小明做加法运算，执行小红做减法运算，结束。
这看起来没什么问题，是的，因为量小。
接着，老师重新宣布，让小明做加法运算，让小红做减法运算，再让小明做乘法运算，再让小红做除法运算。
面向过程设计思路:定义小明做加法运算，定义小明做乘法运算，定义小红做减法运算，定义小红做除法运算。老师发出命令，执行小明做加法运算，执行小红做减法运算，执行小明做乘法运算，执行小红做除法运算。
当然这也看起来很容易，那么接着来，老师让小明做完运算去踢球，再让小红做完题去擦桌子.....
当量开始累积起来是不是开始麻烦起来了？
下面我们来看面向对象的设计方式：
定义小明类：小明做加减乘除运算，小明踢球，小明擦桌子；定义小红类：内容同小明。老师发布命令，调用小明做加法运算，调用小红做减法运算...
结果很明显，面向对象中，我们赋予了对象几乎所有属性，用的时候就调用，而面向过程中，我们用的时候才去定义。当然，在引用继承后，面向对象的编程方式更加要比面向过程简单，这里不再赘述。因此在执行简单操作时，面向过程比面向对象容易，反之，执行复杂程序时，面向对象比面向过程更容易思考。
# 5 怎样理解指针？
传字符串用的。
# 6 类
定义类：
```python
class Student(object):
    
    def __init__(self,name,score):
        self.name=name
        self.score=score
```
注意：`__init__`是两个下划线，是特殊方法。作用是在创建实例的时候，把一些我们认为必须绑定的属性强制填写进去。

实体化类并输出：
```python
xiaoming=Student('xiaomingya',59)
print(xiaoming.name)
```
增加方法：
```python
class Student(object):
    
    def __init__(self,name,score):
        self.name=name
        self.__score=score
    def frint_score(self):
        print('%s:%s'%(self.name,self.score))
        
        


xiaoming=Student('xiaomingya',59)
xiaoming.frint_score()
```
要定义一个方法，除了第一个参数是self外，其他和普通函数一样。
其中，`self.__score=score`为定义私有变量的方式。

# 7 公有变量和私有变量
设置共有变量和私有变量的原因是为了封装，让类看起来更简洁。
把无关的或重要的变量设为私有后，除非修改底层，否则就不能再修改了。
# 8 教程编写
好的教程并不是知无不言言无不尽，而是用到哪讲到哪，不要讲太多，够用就行，全都写出来的是字典而不是教程。
# 9 装饰器
装饰器用于扩展函数功能。举个栗子：
```python
# 定义now函数
def now():
	print('现在是2019年4月6日')
#调用now函数
print(now())
```
输出：
```
现在是2019年4月6日
None
```
下面使用装饰器给now函数增加功能：在调用函数前打印出“调用XX():”字样，比如：“调用now():”。注：以下代码与上边无关。
```python
#定义装饰器
def log(func):
    def wrapper(*args, **kw):
        print('call %s():' % func.__name__)
        return func(*args, **kw)
    return wrapper
#调用装饰器并定义now函数
@log
def now():
    print('2015-3-25')
#调用now函数
print(now())
```
显示如下：
```
call now():
2015-3-25
None
```
其中：
`__name__`是返回函数名的属性；
```
@log
def now():
    print('2015-3-25')
```
相当于
```
now = log(now)
```
# 10 进程和线程
进程是一个任务，线程是进程内的子任务。
有些进程还不止同时干一件事，比如Word，它可以同时进行打字、拼写检查、打印等事情。在一个进程内部，要同时干多件事，就需要同时运行多个“子任务”，我们把进程内的这些“子任务”称为线程（Thread）。
# 11 import
import的作用是导入模块，相当于C语言中的#include
from A import B 的作用是导入A模块中的方法B，而不是将整个A模块导入。
from tkinter import * 是导入tkinter模块的所有方法。
# 12 一个想法
抓取某个页面的新闻，当更新时发送到邮箱。
# 13 列表和字典
列表是数组的高级封装，具备处理数组的能力。
字典和列表（LIST）类似，但不是靠偏移量来取值，而是通过键名来取值。
![image](https://wx2.sinaimg.cn/large/006jQClrly1g225hqk2jtj30qz0dw77j.jpg)
# 14 属性和方法
**私有属性**
`__private_attrs：`两个下划线开头，声明该属性为私有，不能在类的外部被使用或直接访问。在类内部的方法中使用时 `self.__private_attrs`。
**方法**
在类的内部，使用 def 关键字可以为类定义一个方法，与一般函数定义不同，类方法必须包含参数 self,且为第一个参数。
**私有方法**
`__private_method`：两个下划线开头，声明该方法为私有方法，不能在类的外部调用。在类的内部调用 `self.__private_methods`。




# END 杂记
`angle&0xFF`:这相当于将angle由十进制转化为十六进制。
导入 RPi.GPIO 模块。其中as GPIO 指的是：用 GPIO 来表示 RPi.GPIO