---
title: QT-helloworld-QtCreater编写
date: 2019-1-16
categories:
- 笔记
tags:
- qt
img: /medias/paperimg/qt.jpg
toc: true
---
纯代码编写helloworld，解析代码含义。<!-- more -->
# 一、新建空项目

新建->其他项目->Empty qmake Project

# 二、修改.pro文件

打开helloworld.pro文件，添加如下一行代码：

```
greaterThan(QT_MAJOR_VERSION, 4): QT += widgets
```

保存

# 三、新建main.cpp文件

在项目文件夹helloworld上右击，

添加新文件->C++ Source  File->名称main.cpp

添加main文件内容如下：

```c++
#include <QApplication>
#include <QDialog>
#include <QLabel>
 
int main(int argc, char *argv[])
{
    QApplication a(argc, argv);//用于管理应用程序的资源
    QDialog w; //新建一个对话框界面w
    w.resize(400, 300);//对话框大小
    QLabel label(&w); //新建一个label标签，放在对话框窗口w中
    label.move(120, 120);//标签位置，默认左上角是（0,0,）
    label.setText(QObject::tr("Hello World! 你好Qt！"));//label显示的内容,QObject::tr()函数可以实现多语言支持，一般建议程序中所有要显示到界面上的字符串都使用tr()函数括起来
    w.show();//默认情况下，对话框和label是不显示的，因此需要调用显示程序。
    return a.exec();//让QApplication对象进入事件循环，这样当Qt应用程序在运行时便可以接收产生的事件，例如单击和键盘按下等事件。
}
```

保存并运行。

![image](http://upload-images.jianshu.io/upload_images/16115686-53e098df342b9dfe.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


参考自Qt开源社区的Qt学习之路,http://www.qter.org/forum.php?mod=viewthread&amp;tid=629。
