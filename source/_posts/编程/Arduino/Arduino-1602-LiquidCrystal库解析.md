---
title: Arduino-1602-LiquidCrystal库解析
date: 2019-02-05 09:08:00
categories:
- 笔记
tags:
- mcu
toc: true
img: /medias/paperimg/arduino.jpg
summary: LiquidCrystal是LCD1602的IIC库。
author: 陈德强
top: 
cover: false
---


# 一、库函数快速查询

1.  LiquidCrystal()　　　　 //构造函数
2.  begin()　                       //指定显示屏尺寸
3.  clear()　　                    //清屏并将光标置于左上角
4.  home()　                 　 //将光标置于左上角（不清屏）
5.  setCursor()　　           //将光标置于指定位置
6.  write()　　                   //（在光标处）显示一个字符
7.  print()　                  　 //显示字符串
8.  cursor()　                　//显示光标（就是一个下划线）
9.  noCursor()　　           //不显示光标
10.  blink()　                  　   //光标闪烁（和8,9一起使用时不保证效果）
11.  noBlink()　　                //光标不闪烁
12.  noDisplay()　　            //关闭显示，但不会丢失内容
13.  display()　　                 //（使用noDisplay后）恢复显示 
14.  scrollDisplayLeft()　     //将显示的内容向左滚动一格
15.  scrollDisplayRight() 　  //将显示的内容向右滚动一格
16.  autoscroll()　　            //打开自动滚动
17.  noAutoscroll()　       　//关闭自动滚动
18.  leftToRight()　　         //从左向右显示内容（默认）
19.  rightToLeft()　　         //从右向左显示内容
20.  createChar()　　         //创造字符

#  二、库函数详细释义

## 2.1 LiquidCrystal()

### 内容：

　　构造函数，创建一个LiquidCrystal的实例（LiquidCrystal是一个类）。可使用4线或8线方式作为数据线(请注意,还需要指令线).若采用四线方式,将d0-d3悬空不连接.RW引脚可接地而不用接在Arduino的某个引脚上;如果这样接,省略在函数中的rw参数.

### 语法：

--------------------------------------------------------

LiquidCrystal(rs, enable, d4, d5, d6, d7) 
LiquidCrystal(rs, rw, enable, d4, d5, d6, d7) 
LiquidCrystal(rs, enable, d0, d1, d2, d3, d4, d5, d6, d7) 
LiquidCrystal(rs, rw, enable, d0, d1, d2, d3, d4, d5, d6, d7)

------------------------------------

### 参数设置：

----------------------------

　　　　rs: rs连接的Arduino的引脚编号 
　　　　rw: rw连接的Arduino的引脚编号 
　　　　enable:enable连接的Arduino的引脚编号 
　　　　d0, d1, d2, d3, d4, d5, d6, d7: 连接的Arduino的引脚编号

------------------------------------


### 例子：

```
#include <LiquidCrystal.h>
 
LiquidCrystal lcd(12, 11, 10, 5, 4, 3, 2);    
 
void setup()
{
    lcd.print("hello, world!");
}
 
void loop() {}
```

## 2.2  begin ()

### 内容:

　　　　指定显示屏的尺寸（宽度和高度）。

### 语句:

　　　　lcd.begin(cols, rows)

### 参数设置：

　　　　lcd: 液晶类型的名称变量 
　　　　cols: 显示器可以显示的列数(1602是16列) 
　　　　rows: 显示器可以显示的行数(1602是2行)

## 2.3  clear ()

### 简介：

　　　　清除LCD屏幕上内容,并将光标置于左上角。

### 语法：

　　　　lcd.clear()

### 参数：

　　　　lcd：LiquidCrystal类的对象

## 2.4  home()

### 内容：

　　　　将光标定位在屏幕左上角. 就是说,接下来的字符从屏幕左上角开始显示.如果同时要清除屏幕上的内容,请使用clear()函数代替.

### 语法：

　　　　lcd.home()

### 参数设置：

　　　　lcd: LiquidCrystal类的对象

## 2.5 setCursor()

### 简介：

　　　　将光标定位在特定的位置。

### 语法：

　　　　lcd.setCursor(col, row)

### 参数：

　　　　lcd：LiquidCrystal类的对象
　　　　col: 你要显示光标的列 (从0开始计数) 
　　　　row: 你要显示光标的行 (从0开始计数)

## 2.6 write()

### 简介：

　　　　向LCD写一个字符。

### 语法：

　　　　lcd.write(data)

### 参数：

　　　　lcd: LiquidCrystal类的对象 
　　　　data: 你要显示的字符（仅限英文、数字和自定义字符）。

### 返回值：

　　　　byte   　　//write() 将返回写入的字节数，虽然读这个数字是可选

### 示例：

 ```
#include <LiquidCrystal.h>
 
LiquidCrystal lcd(12, 11, 10, 5, 4, 3, 2);
 
void setup()
{
    Serial.begin(9600);
}
 
void loop()
{
    if (Serial.available()) 
    {
        lcd.write(Serial.read());
    }
}
```

## 2.7  print()

### 内容：

　　　　将文本显示在LCD上.

### 语法：

　　　　lcd.print(data) 
　　　　lcd.print(data, BASE)

### 参数：

　　　　lcd: 液晶类型的名称变量 
　　　　data:要显示的数据,可以是char, byte, int, long或者string类型的 
　　　　BASE (optional): 数制(可选),BIN,DEC,OCT,HEX分别将数字以二进制,十进制,八进制,十六进制方式显示出来.

### 返回值：

　　　　byte     //这个返回值通常是用不到的

### 示例：

```
#include <LiquidCrystal.h>
 
LiquidCrystal lcd(12, 11, 10, 5, 4, 3, 2);
 
void setup()
{
    lcd.print("hello, world!");
}
 
void loop() {}
```

## 2.8  cursor()

### 内容：

　　　　显示光标(光标所在的位置, 就是下一个字符将会被显示的位置)。

### 语法：

　　　　lcd.cursor()

### 参数设置：

　　　　lcd: 液晶类型的名称变量

## 2.9  noCursor()

### 内容：

　　　　隐藏光标。

### 语法：

　　　　lcd.noCursor()

### 参数：

　　　　lcd: 液晶类型的名称变量

## 2.10 blink()

### 内容：

　　　　显示闪烁的光标。如果和cursor()一起使用,最终结果将取决于您使用的LCD屏幕.

### 语法：

　　　　lcd.blink()

### 参数设置：

　　　　lcd: 液晶类型的名称变量

## 2.11 noBlink()

### 内容：

　　　　关闭 光标闪烁功能.

### 语句：

　　　　lcd.noBlink()

### 参数设置：

　　　　lcd: 液晶类型的名称变量

## 2.12 noDisplay()

### 内容：

　　　　关闭液晶显示,但原先显示的内容不会丢失. 可使用display()恢复显示.

### 语法：

　　　　lcd.noDisplay()

### 参数：

　　　　lcd: 液晶类型的名称变量

## 2.13 display()

### 简介：

　　　　调用noDisplay()隐藏LCD上显示内容后,调用本函数恢复显示.

### 语法：

　　　　lcd.display()

### 参数：

　　　　lcd: 液晶类型的名称变量

## 2.14 scrollDisplayLeft()

### 简介：

　　　　使屏幕上内容(光标及文字)向左滚动一个字符。

### 语法：

　　　　lcd.scrollDisplayLeft()

### 参数：

　　　　lcd: 一个LiquidCrystal类的对象

## 2.15 scrollDisplayRight()

### 简介：

　　　　使屏幕上内容(光标及文字)向右滚动一个字符。

### 语法：

　　　　lcd.scrollDisplayRight()

### 参数:

　　　　lcd: 一个LiquidCrystal类的对象

## 2.16 autoscroll()

### 简介：

　　打开液晶显示屏的自动滚动,将会使得当一个字符输出到LCD时,令先前的文本移动一个位置.如果当前写入方向为由左到右(默认方向),文本向左滚动.反之,文本向右滚动.它的功能可以理解为,当输出单个字符时,会使得字符总是输出在LCD上的同一个位置.

### 语法：

　　　　lcd.autoscroll()

### 参数：

　　　　lcd: a variable of type LiquidCrystal

## 2.17 noAutoscroll()

### 简介：

　　　　关闭自动滚动功能。（后输入的字符可能无法显示）

### 语法：

　　　　lcd.noAutoscroll()

### 参数：

　　　　LCD：LiquidCrystal类的对象

## 2.18 leftToRight()

### 内容：

　　　　默认的方向,将文本从左到右写入屏幕.这意味着,后续字符的显示将是从左向右的,但是这不会影响先前已经显示的字符.

### 语法：

　　　　lcd.leftToRight()

### 参数设置：

　　　　lcd: a variable of type LiquidCrystal

## 2.19 rightToLeft()

### 简介：

　　　　设置文本写入LCD的方向为从右向左（默认是从左向右）。这意味着，后续字符将会由右至左写入，但不影响先前的文本的显示。

### 语法：

　　　　lcd.rightToLeft()

### 参数：

　　　　lcd: 一个LiquidCrystal类的对象

## 2.20 createChar()

### 内容：

　　创建用户自定义的字符.共可创建8个用户自定义字符,编号从0到7.字符外观由一个8字节数组定义,每行占用一个字节.最低的5个有效位决定像素点所在的行.若要在屏幕显示自定义字符,请使用write()函数.(参数为字符的编号0-7)

### 语法：

　　　　lcd.createChar(num, data)

### 参数设置：

　　　　lcd: a variable of type LiquidCrystal 
　　　　num: 所创建字符的编号(0-7) 
　　　　data: 字符的像素数据

### 例子：

```
#include <LiquidCrystal.h>
 
LiquidCrystal lcd(12, 11, 5, 4, 3, 2);
 
byte smiley[8] = {    //1表示亮，0表示不亮，此例显示一个笑脸
    B00000,
    B10001,
    B00000,
    B00000,
    B10001,
    B01110,
    B00000,
};
 
void setup() {
    int x=1;    //x可以为0~7的任何数字
    lcd.createChar(x , smiley);    //将x号字符设置为smiley数组表示的样子
    lcd.begin(16, 2);  
    lcd.write（x）;
}
 
void loop() {}
```

参考自:

1. https://www.cnblogs.com/Dumblidor/p/5394302.html

2. https://wenku.baidu.com/view/f0551a3deefdc8d376ee327d.html
