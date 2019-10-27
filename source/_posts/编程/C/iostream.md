---
title: iostream
author: 陈德强
date: 2019-09-26 09:28:00
categories:
- 字典
tags:
- CPlus
toc: true
top: false
img: /medias/paperimg/c.jpg
summary: iostream
---

包含的文件
```c
<ios>	
<streambuf>	
<istream>	
<ostream>	
```

包含的对象
```c
std::cin     标准输入
std::cout    标准输出
std::cerr    标准错误
std::clog    标准日志
std::wcin    标准输入
std::wcout   标准输出
std::wcerr   标准错误
std::wclog   标准日志
```

重定向。标准输入和输入通常连接着键盘和屏幕，很多操作系统都支持重定向，如Win、Linux和Unix。


# cout

星号*为默认

|控制符|	描 述|
|:----:|:---:|
|*dec|	以十进制形式输出整数	|
|hex|	以十六进制形式输出整数|
|oct	|以八进制形式输出整数|
|fixed|	以普通小数形式输出浮点数|
|scientific|	以科学计数法形式输出浮点数|
|left	|左对齐，即在宽度不足时将填充字符添加到右边|
|*right	|右对齐，即在宽度不足时将填充字符添加到左边|
|setbase(b)	|设置输出整数时的进制，b=8、10 或 16|
|setw(w)|	指定输出宽度为 w 个字符，或输人字符串时读入 w 个字符|
|setfill(c)|	在指定输出宽度的情况下，输出的宽度不足时用字符 c 填充（默认情况是用空格填充）|
|setprecision(n)|	设置输出浮点数的精度为 n。在使用非 fixed 且非 scientific 方式输出的情况下，n 即为有效数字最多的位数，如果有效数字位数超过 n，则小数部分四舍五人，或自动变为科学计 数法输出并保留一共 n 位有效数字。在使用 fixed 方式和 scientific 方式输出的情况下，n 是小数点后面应保留的位数。|
|setiosflags(flag)|	将某个输出格式标志置为 1|
|resetiosflags(flag)	|将某个输出格式标志置为 0|
|boolapha	|把 true 和 false 输出为字符串	|
|*noboolalpha	|把 true 和 false 输出为 0、1|
|showbase	|输出表示数值的进制的前缀|
|*noshowbase|	不输出表示数值的进制.的前缀|
|showpoint	|总是输出小数点|
|*noshowpoint	|只有当小数部分存在时才显示小数点|
|showpos	|在非负数值中显示 +|
|*noshowpos	|在非负数值中不显示 +|
|*skipws	|输入时跳过空白字符|
|noskipws	|输入时不跳过空白字符|
|uppercase	|十六进制数中使用 A~E。若输出前缀，则前缀输出 0X，科学计数法中输出 E|
|*nouppercase	|十六进制数中使用 a~e。若输出前缀，则前缀输出 0x，科学计数法中输出 e。|
|internal	|数值的符号（正负号）在指定宽度内左对齐，数值右对 齐，中间由填充字符填充。|



## 栗子
```c
#include<iostream>

using namespace std;

int main() {
	
   int a = 10;
   cout << dec << a << endl;
   cout << hex << a << endl;
   cout << a << endl;
   system("pause");
   return 0;
}
```
```
10
a
a
```

# cin
cin是C++编程语言中的标准输入流对象，即istream类的对象。cin主要用于从标准输入读取数据，这里的标准输入，指的是终端的键盘。此外，cout是流的对象，即ostream类的对象，cerr是标准错误输出流的对象，也是ostream 类的对象。这里的标准输出指的是终端键盘，标准错误输出指的是终端的屏幕。

在理解cin功能时，不得不提标准输入缓冲区。当我们从键盘输入字符串的时候需要敲一下回车键才能够将这个字符串送入到缓冲区中，那么敲入的这个回车键(\r)会被转换为一个换行符\n，这个换行符\n也会被存储在cin的缓冲区中并且被当成一个字符来计算！比如我们在键盘上敲下了123456这个字符串，然后敲一下回车键（\r）将这个字符串送入了缓冲区中，那么此时缓冲区中的字节个数是7 ，而不是6。

cin读取数据也是从缓冲区中获取数据，缓冲区为空时，cin的成员函数会阻塞等待数据的到来，一旦缓冲区中有数据，就触发cin的成员函数去读取数据。

## cin>>
cin可以连续从键盘读取想要的数据，以空格、tab或换行作为分隔符。
```c
cin>>a>>b>>c;
```
cin>>对缓冲区中的第一个换行符视而不见，采取的措施是忽略清除，继续阻塞等待缓冲区有效数据的到来。

```c
getline(cin,test);//不阻塞
```
但是，getline()读取数据时，并非像cin>>那样忽略第一个换行符，getline()发现cin的缓冲区中有一个残留的换行符，不阻塞请求键盘输入，直接读取，送入目标字符串后，再将换行符替换为空字符’\0’，因此程序中的test为空串。

## cin.get
该函数有有多种重载形式，分为四种格式：无参，一参数，二参数，三个参数。
```c
int cin.get();
istream& cin.get(char& var);
istream& get ( char* s, streamsize n );
istream& get ( char* s,  streamsize  n, char delim );
```
### 读取字符
```c
char b;
cin.get(b);
```
### 读取一行
读取一行可以使用istream& get ( char* s, streamsize n )或者istream& get ( char* s, size_t n, streamsize delim )。二者的区别是前者默认以换行符结束，后者可指定结束符。n表示目标空间的大小。
```c
char array[20]={NULL}; 
cin.get(array,20);
```
cin.get(array,20);读取一行时，遇到换行符时结束读取，但是不对换行符进行处理，换行符仍然残留在输入缓冲区。第二次由cin.get()将换行符读入变量,这也是cin.get()读取一行与使用getline读取一行的区别所在。getline读取一行字符时，默认遇到’\n’时终止，并且将’\n’直接从输入缓冲区中删除掉，不会影响下面的输入处理。
cin.get(str,size);读取一行时，只能将字符串读入C风格的字符串中，即char*，但是C++的getline函数可以将字符串读入C++风格的字符串中，即string类型。鉴于getline较cin.get()的这两种优点，建议使用getline进行行的读取。

## cin.getline
从标准输入设备键盘读取一串字符串，并以指定的结束符结束。 
有两种形式：
```c
istream& getline(char* s, streamsize count); //默认以换行符结束
istream& getline(char* s, streamsize count, char delim);
```
举个栗子：
```c
char array[20]={NULL};
cin.getline(array,20); //或者指定结束符，使用下面一行
//cin.getline(array,20,'\n');
```
注意，cin.getline与cin.get的区别是，cin.getline不会将结束符或者换行符残留在输入缓冲区中。

# 文件流
## 文件存取方式
### 测试
包括顺序存取方式和随机存取方式两种。
顺序读取也就是从上往下，一笔一笔读取文件的内容。保存数据时，将数据附加在文件的末尾。这种存取方式常用于文本文件，而被存取的文件则称为顺序文件。
随机存取方式多半以二进制文件为主。它会以一个完整的单位来进行数据的读取和写入，通常以结构为单位。

## 文本文件操作

C语言中主要通过标准I/O函数来对文本文件进行处理。相关的操作包括打开、读写、关闭与设置缓冲区。
相关的存取函数有：fopen(), fclose(), fgetc(), fputc(), fgets(), fputs(), fprintf(), fscanf()等。

## 写入文件

ifstream

| 常量 | 解释 |
|:---:|:---:|
| app | 每次写入前寻位到流结尾 |
| binary | 以二进制模式打开 |
| in | 为读打开 |
| out| 为写打开 |
| trunc | 在打开时舍弃流的内容 |
| ate | 打开后立即寻位到流结尾 |

```c
ifstream is(dataFileName, ios_base::in | ios_base::binary);
```

## 打开文件

| 参数 | 解释 |
|:---:|:---:|
r|只能从文件中读数据，该文件必须先存在，否则打开失败
w|只能向文件写数据，若指定的文件不存在则创建它，如果存在则先删除它再重建一个新文件
a|向文件增加新数据(不删除原有数据)，若文件不存在则打开失败，打开时位置指针移到文件末尾
r+|可读/写数据，该文件必须先存在，否则打开失败
w+|可读/写数据，用该模式打开新建一个文件，先向该文件写数据，然后可读取该文件中的数据
a+|可读/写数据，原来的文件不被删去，位置指针移到文件末尾

```c
FILE *fp;
fp = fopen("c:\\temp\\test.txt", "r") //由于反斜杠\是控制字符，所以必须再加一个反斜杠
```
参考链接：
https://blog.csdn.net/bravedence/article/details/77282039
https://blog.csdn.net/tjcwt2011/article/details/81125912