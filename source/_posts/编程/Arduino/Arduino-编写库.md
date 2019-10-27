---
title: Arduino-编写库
date: 2019-02-05
categories:
- 教程
tags:
- mcu
toc: true
img: /medias/paperimg/arduino.jpg
---

一开始写Arduino 的时候很不习惯，没有main函数，因为好多东西都被隐藏了。一直想搞清楚，以便编写自己的库文件。于是研究一下午，下面是一些总结。<!-- more -->

# Arduino工程的初步认识

**一、目录规范**

当创建一个空的工程，先按下ctrl+s保存一下。这个时候弹出对话框，命名工程。假如命名为LED，并保存在 我自己的Arduino工作目录下  H:\Arduino\workspace\。

于是IDE会自动帮我们在workspace下创建1个文件夹，并将sketch主文件放在里面，而且主文件和文件夹同名。

 H:\Arduino\workspace\ LED\ LED.ino

**二、主文件代码框架规范**

每一个Arduino程序（Sketch）都有1个主文件，后缀为 **.ino** ，它是程序的setup 函数和 loop函数所在的文件。

代码框架如下：

```
void setup() {
  // put your setup code here, to run once:
  //初始化操作代码放在setup函数中，他们将在程序启动的第一步得到执行 并只执行一次
}

void loop() {
  // put your main code here, to run repeatedly:
  //将程序的主要逻辑代码，放在loop里。他们将会反复执行下去。
}
```

有C/C++开发经验的人看到这个程序框架会愣住：**我的main函数去哪里呢？**

Arduino  为了让更多的人能够使用Arduino平台开发出好玩的东西出来，绞尽脑汁降低门槛，它隐藏了程序的细节，使得开发者将注意力放在实现上。

在Arduino IDE的安装目录下可以找到main.cpp这个代码模板文件，main函数就位于此。文件位置：{Arduino安装目录}\hardware\arduino\avr\cores\arduino\main.cpp，内容如下：

```
/*
  main.cpp - Main loop for Arduino sketches
  Copyright (c) 2005-2013 Arduino Team.  All right reserved.

  This library is free software; you can redistribute it and/or
  modify it under the terms of the GNU Lesser General Public
  License as published by the Free Software Foundation; either
  version 2.1 of the License, or (at your option) any later version.

  This library is distributed in the hope that it will be useful,
  but WITHOUT ANY WARRANTY; without even the implied warranty of
  MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU
  Lesser General Public License for more details.

  You should have received a copy of the GNU Lesser General Public
  License along with this library; if not, write to the Free Software
  Foundation, Inc., 51 Franklin St, Fifth Floor, Boston, MA  02110-1301  USA
*/

#include <Arduino.h>

// Declared weak in Arduino.h to allow user redefinitions.
int atexit(void (* /*func*/ )()) { return 0; }

// Weak empty variant initialization function.
// May be redefined by variant files.
void initVariant() __attribute__((weak));
void initVariant() { }

void setupUSB() __attribute__((weak));
void setupUSB() { }

int main(void)
{
    init();        //硬件初始化

    initVariant();  //特有硬件初始化。因为不同的开发板有自己独特的初始化逻辑。

#if defined(USBCON)
    USBDevice.attach();
#endif
    
    setup();
    
    for (;;) {
        loop();
        if (serialEventRun) serialEventRun();
    }
        
    return 0;
}
```

## 在项目中使用多文件

有时会程序越写越大，越大越乱。多文件管理可以解决这个麻烦。Arduino程序可以有多个源代码文件，但只有 1个 主文件，也就是存放 setup、loop函数的.ino文件。

为了使得代码更清晰，我们让主文件用来控制程序的主要逻辑部分，而把具体的细节封装成单个模块，存放在其他的文件中，这样方便管理。

那么怎么创建其他的文件呢？？？下面开始介绍。

**使用无后缀的文件**（其实是以.ino为后缀的，只是在IDE中不会显示后缀，而在电脑的资源管理器中会显示.ino  ， 以下都称为无后缀）

点击下图中标记的按钮，选择第一个选项 【新建标签】，输入文件名即可。

![image](http://upload-images.jianshu.io/upload_images/16115686-f3054ed12e7a0d01.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这样我们的工程就有了2个文件了。如下，一个主文件和一个名为LED的文件。这就是最简单的多文件方法。

![image.png](https://upload-images.jianshu.io/upload_images/16115686-7c528d14d30dd250.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我不推荐使用这种方法，这是为没有C/C++编程经验的小白准备的，他们不懂函数定义 后还要声明才能使用，不懂得头文件的包含。这些都被Arduino IDE帮他们做了。IDE的具体处理是

在编译前期，Arduino IDE会将无后缀的文件 和 主文件合并成为1个文件，效果就像是写在主文件中一样。并在主文件第一行添加  #include "Arduino.h" 。 Arduino.h是 Arduino程序的核心头文件。然后，IDE将扫描合并后文件的函数定义，并对已经定义的函数添加函数的声明。（这个就是为什么即便我们定义的函数不声明也能编译通过的原因了）

但是官方明确说了，这个自动插入函数声明的机制是不完美的！所以我也建议大家养成手动声明函数的习惯。

> Also, this generation isn't perfect: it won't create prototypes for functions that have default argument values, or which are declared within a namespace or class.

**使用传统的 C/C++分离式文件**

这种方式下，对于一个代码模块，我们需要一对文件：源文件和头文件，即： .c  和.h   或者 .cpp 和 .h  。前者是C语言风格，后者是对会使用C++来说的。官方貌似推崇我们使用C++编写Arduino代码，无论是Arduino 的从标准库，还是教程中，都透露出一股强烈的OOP气息。所以我下面使用C++风格来举例子。

例如我们想要将LED的控制封装成一个模块。一开始我们需要创建2个文件 ：LED.h   、 LED.cpp

 ![image](http://upload-images.jianshu.io/upload_images/16115686-4dacd44d68a31f58.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后是想清楚我们需要让提供LED控制的哪些操作。发挥你的想象力时候到了。规定操作后，我们先写出头文件，然后写出实现，最后在主文件中使用这个模块。在主文件中使用

#include"LED.h"预处理指令包含。

```
/*******************
LED.h

*******************/

#ifndef _LED_H__
#define _LED_H__

//导入Arduino核心头文件
#include"Arduino.h"  


class LED
{
     private:
          byte pin;        //控制led使用的引脚
     
     
     public:
          
          LED(byte p , bool state=LOW );   //构造函数
          
          ~LED();          //析构函数

          byte getPin();   //获取控制的引脚
          
          void on();      //打开LED

          void off();     //关闭LED

          bool getState();  //获取LED状态
          void disattach(); //释放引脚与LED的绑定，使得引脚可以控制其他的东西

};


#endif
```

----------------------------


```
/*****************
LED.cpp

******************/

#include"LED.h"
#include"Arduino.h"


LED::LED(byte p,bool state):pin(p)
{
   
   pinMode(pin,OUTPUT);
   digitalWrite(pin,state);
}

LED::~LED()
{
    disattach();
} 

         
void LED::on()
{
    digitalWrite(pin,HIGH);
}

void LED::off()
{
   digitalWrite(pin,LOW);
}

bool LED::getState()
{
    return digitalRead(pin);
}

void LED::disattach()        //引脚回收，恢复到上电状态
{
    digitalWrite(pin,LOW);
    pinMode(pin,INPUT);
}
```

## 让它成为你自己的库！

如果上面的模块你觉得好用，符合自己的使用习惯，而且经常要用到，那么你可以将它变成你自己的库文件。这样以后就可以直接拿来用啦。

Arduino的扩展库都是放在 libraries目录下的。

![image](http://upload-images.jianshu.io/upload_images/16115686-bcaef8386b6b32d4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

所以我们需要在这个目录下创建一个文件夹，比如上面的例子是LED控制，于是我创建了 m_LED文件夹（前面加m是为了和官方库区分开，这只是我自己的习惯而已）。然后把写好的.cpp 和 .h文件拷贝到里面去，这样就OK了。

![image](http://upload-images.jianshu.io/upload_images/16115686-32e771c0a212da65.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这样我们 的主文件就变成了下面这样，是不是很简洁干净呢。

```
#include<LED.h>     
                 //注意，由于LED控制模块已经是标准库了，所以使用尖括号<> 包含

LED led(7);
byte count =0;

void setup() {
   Serial.begin(9600);
}

void loop() {

   if(count<10){
     led.on();
     delay(300);
     Serial.println(led.getState(),DEC);
     
     led.off();
     delay(300);
     Serial.println(led.getState(),DEC);
     
     ++count;
     if(count==10)
        led.disattach();
   }
}
```

细心的同学会发现 和 LED.cpp  、 LED.h 一起有个 keywords.txt文件，这个是什么用呢？ 其实它没有太大的实用性，只是为了配置自定义库的语法高亮。让我们自己的库能在IDE下显示不同的颜色而已。如果不配置，Arduino IDE不能渲染出颜色的。

 ![image.png](https://upload-images.jianshu.io/upload_images/16115686-964bf156794ab5fc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

下面是keywords.txt 的内容，其中#开头的是注释，完全可以不写。格式：word【tab】DESCRIPTION

word就是你要高亮的关键字接着1 个 tab 键 ，然后就是DESCRIPTION。

DESCRIPTION可以取的值：

KEYWORD1    高亮类名

KEYWORD2    高亮方法名

LITERAL1       高亮常量

注意中间使用的是 1  个  tab 键 隔开的

#class (KEYWORD1)

LED    KEYWORD1

#function and method (KEYWORD2)

on    KEYWORD2

off    KEYWORD2

getState    KEYWORD2

disattach    KEYWORD2

#constant (LITERAL1)

#none

转载自：[http://www.cnblogs.com/lulipro/p/6090407.html](http://www.cnblogs.com/lulipro/p/6090407.html)

