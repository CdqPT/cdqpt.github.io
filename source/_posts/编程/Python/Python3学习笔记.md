---
title: Python3学习笔记
author: 陈德强
date: 2019-06-25 20:04:00
categories:
- 笔记
tags:
- python
toc: true
top: false
img: /medias/paperimg/python.jpg
summary: Python3学习笔记。
---

1. python中保留的关键字
['False', 'None', 'True', 'and', 'as', 'assert', 'break', 'class', 'continue', 'def', 'del', 'elif', 'else', 'except', 'finally', 'for', 'from', 'global', 'if', 'import', 'in', 'is', 'lambda', 'nonlocal', 'not', 'or', 'pass', 'raise', 'return', 'try', 'while', 'with', 'yield']

2. Python解释器
在linux系统下，一般py3.0的安装路径是`/usr/local/python3`
在Linux和Unix系统中，添加`#! /usr/bin/env python3`就可以在终端执行脚本了。
```python
$ python3 test.py 
```

3. is和==的区别
is比较的是存储地址，==比较的是值。

4. 查询python的安装路径
在终端输入
```
which python
```
可得到结果：
```
/anaconda3/bin/python
```
5. 列表
列表不等于数组，因为列表中的数据项不需要相同的类型，而且列表可以切片，运算，检查等。
```python
#!/usr/bin/python3
 
list1 = ['Google', 'Runoob', 1997, 2000];
list2 = [1, 2, 3, 4, 5, 6, 7 ];
 
print ("list1[0]: ", list1[0])
print ("list2[1:5]: ", list2[1:5])
```
```
list1[0]:  Google
list2[1:5]:  [2, 3, 4, 5]
```

6. 元祖
元祖和列表类似，区别是元祖的数据不可以通过赋值运算符修改，但可以进行一些运算。
```python
tup1 = ('Google', 'Runoob', 1997, 2000);
```
7. 字典
列表是靠序列（即0，1，2，3....）来索引的，但字典是靠键值来索引的。

```python
dict = {'Name': 'Runoob', 'Age': 7, 'Class': 'First'}
#修改键值
dict['Age'] = 8               # 更新 Age
dict['School'] = "菜鸟教程"  # 添加信息
```
遍历字典

```python
>>> knights = {'gallahad': 'the pure', 'robin': 'the brave'}
>>> for k, v in knights.items():
...     print(k, v)
```
```
gallahad the pure
robin the brave
```
8. 集合
集合（set）是一个无序的不重复元素序列。
创建一个空集合必须用 set()。
```python
>>>basket = {'apple', 'orange', 'apple', 'pear', 'orange', 'banana'}
>>> print(basket)                      # 这里演示的是去重功能
{'orange', 'banana', 'pear', 'apple'}
>>> 'orange' in basket                 # 快速判断元素是否在集合内
True
>>> 'crabgrass' in basket
False
 
>>> # 下面展示两个集合间的运算.
...
>>> a = set('abracadabra')
>>> b = set('alacazam')
>>> a                                  
{'a', 'r', 'b', 'c', 'd'}
>>> a - b                              # 集合a中包含而集合b中不包含的元素
{'r', 'd', 'b'}
>>> a | b                              # 集合a或b中包含的所有元素
{'a', 'c', 'r', 'd', 'b', 'm', 'z', 'l'}
>>> a & b                              # 集合a和b中都包含了的元素
{'a', 'c'}
>>> a ^ b                              # 不同时包含于a和b的元素
{'r', 'd', 'b', 'm', 'z', 'l'}
```

9. 关键字end
关键字end可以用于将结果输出到同一行，或者在输出的末尾添加不同的字符，实例如下：

```python
#!/usr/bin/python3
 
# Fibonacci series: 斐波纳契数列
# 两个元素的总和确定了下一个数
a, b = 0, 1
while b < 1000:
    print(b, end=',')
    a, b = b, a+b
```

```
1,1,2,3,5,8,13,21,34,55,89,144,233,377,610,987,
```

10. for循环
for循环在python中主要用于遍历任何序列的项目，如一个列表或者一个字符串。

```python
#!/usr/bin/python3
 
sites = ["Baidu", "Google","Runoob","Taobao"]
for site in sites:
    if site == "Runoob":
        print("菜鸟教程!")
        break
    print("循环数据 " + site)
else:
    print("没有循环数据!")
print("完成循环!")
```

```
循环数据 Baidu
循环数据 Google
菜鸟教程!
完成循环!
```

*range()函数*
如果你需要遍历数字序列，可以使用内置range()函数。它会生成数列。

```python
>>>for i in range(5):
...     print(i)
```

```
0
1
2
3
4
```
```python
>>>for i in range(0, 10, 3) :
    print(i)
```
```
0
3
6
9
```
更多细节请参考：[https://www.runoob.com/python3/python3-loop.html](https://www.runoob.com/python3/python3-loop.html)

11. 迭代器
迭代器能够用来遍历标准模板库容器中的部分或全部元素，无需关心容器的内容。

```python
#!/usr/bin/python3
 
list=[1,2,3,4]
it = iter(list)    # 创建迭代器对象
for x in it:
    print (x, end=" ")
```

```
1 2 3 4
```

12. global关键字
当内部作用域想修改外部作用域的变量时，就要用到global和nonlocal关键字了。

```python
#!/usr/bin/python3
 
num = 1
def fun1():
    global num  # 需要使用 global 关键字声明
    print(num) 
    num = 123
    print(num)
fun1()
print(num)
```
```
1
123
123
```

13. 列表
Python中列表是可变的，这是它区别于字符串和元组的最重要的特点，一句话概括即：列表可以修改，而字符串和元组不能。

```python
>>> a = [66.25, 333, 333, 1, 1234.5]
>>> print(a.count(333), a.count(66.25), a.count('x'))
2 1 0
>>> a.insert(2, -1)
>>> a.append(333)
>>> a
[66.25, 333, -1, 333, 1, 1234.5, 333]
>>> a.index(333)
1
>>> a.remove(333)
>>> a
[66.25, -1, 333, 1, 1234.5, 333]
>>> a.reverse()
>>> a
[333, 1234.5, 1, 333, -1, 66.25]
>>> a.sort()
>>> a
[-1, 1, 66.25, 333, 333, 1234.5]
```

14. python数据结构介绍与区别
列表：可更改的序列内容。
元祖：不可更改的序列内容，但是可以运算。
集合：不重复内容的序列内容。
字典：靠关键字检索的序列内容。

15. python 模块
所谓模块就是一个.py文件。
import：导入模块；
from … import ：导入模块中的指定部分；
from … import * ：把一个模块中的全部内容导入；

16. __name__属性

 每个模块都有一个__name__属性，当其值是'__main__'时，表明该模块自身在运行，否则是被引入。下面这条语句表明在执行自身文件时才会执行，否则不执行。

```python
#!/usr/bin/python3
# Filename: using_name.py

if __name__ == '__main__':
   print('程序自身在运行')
else:
   print('我来自另一模块')
```
```
$ python using_name.py
程序自身在运行

$ python
>>> import using_name
我来自另一模块
>>>
```

17. dir() 函数

内置的函数 dir() 可以找到模块内定义的所有名称。以一个字符串列表的形式返回:

```python
>>> import fibo, sys
>>> dir(fibo)
['__name__', 'fib', 'fib2']
>>> dir(sys)  
['__displayhook__', '__doc__', '__excepthook__', '__loader__', '__name__',
 '__package__', '__stderr__', '__stdin__', '__stdout__',
 '_clear_type_cache', '_current_frames', '_debugmallocstats', '_getframe',
 '_home', '_mercurial', '_xoptions', 'abiflags', 'api_version', 'argv',
 'base_exec_prefix', 'base_prefix', 'builtin_module_names', 'byteorder',
 'call_tracing', 'callstats', 'copyright', 'displayhook',
 'dont_write_bytecode', 'exc_info', 'excepthook', 'exec_prefix',
 'executable', 'exit', 'flags', 'float_info', 'float_repr_style',
 'getcheckinterval', 'getdefaultencoding', 'getdlopenflags',
 'getfilesystemencoding', 'getobjects', 'getprofile', 'getrecursionlimit',
 'getrefcount', 'getsizeof', 'getswitchinterval', 'gettotalrefcount',
 'gettrace', 'hash_info', 'hexversion', 'implementation', 'int_info',
 'intern', 'maxsize', 'maxunicode', 'meta_path', 'modules', 'path',
 'path_hooks', 'path_importer_cache', 'platform', 'prefix', 'ps1',
 'setcheckinterval', 'setdlopenflags', 'setprofile', 'setrecursionlimit',
 'setswitchinterval', 'settrace', 'stderr', 'stdin', 'stdout',
 'thread_info', 'version', 'version_info', 'warnoptions']
```












