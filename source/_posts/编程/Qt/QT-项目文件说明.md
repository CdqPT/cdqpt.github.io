---
title: QT-项目文件说明
date: 2019-1-16
categories:
- 笔记
tags:
- qt
toc: true
img: /medias/paperimg/qt.jpg
---
QT-项目文件说明。<!-- more -->
# 一、项目文件概述
|文件|	功能|
|:---:|:---:|
|helloworld.pro|	包含了项目信息|
|helloworld.pro.user	|用户信息|
|hellodialog.h	|自定义类hellodialog的头文件|
|hellodialog.cpp	|自定义类hellodialog的原文件|
|main.cpp|	主函数|
|hellodialog.ui|	图形设计界面|
# 二、helloworld.pro文件说明
```
#-------------------------------------------------
#
# Project created by QtCreator 2019-01-16T12:52:38
#
#-------------------------------------------------
 
QT       += core gui
 
greaterThan(QT_MAJOR_VERSION, 4): QT += widgets
 
TARGET = HelloDialog
TEMPLATE = app
 
 
SOURCES += main.cpp\
        hellodialog.cpp
 
HEADERS  += hellodialog.h
 
FORMS    += hellodialog.ui
 
RC_ICONS = ball.ico
```

代码说明：

-----------
第7行表明了这个项目使用的模块。core模块包含了Qt的核心功能，其他所有模块都依赖于这个模块；而gui模块提供了窗口系统集成、事件处理、OpenGL和OpenGL ES集成、2D图形、基本图像、字体和文本等功能。当使用qmake工具来构建项目时，core模块和gui模块是被默认包含的，这也是为什么前面手动编写项目文件时不添加这两个模块也可以编译的原因。其实所谓的模块，就是很多相关类的集合，读者可以在Qt帮助中查看Qt Core和Qt GUI关键字。
第9行添加了widgets模块，在Qt Widgets模块中提供了经典的桌面用户界面的UI元素集合，简单来说，所有C++程序用户界面部件都在该模块中。
第11行是生成的目标文件的名称，就是生成的exe文件的名字，默认的是项目的名称，当然也可以在这里改为别的名称。第12行使用app模板，表明这是个应用程序。
第15，18和20行分别是工程中包含的源文件、头文件和界面文件。
第22行就是添加的应用程序图标。这里这些文件都使用了相对路径，因为都在项目目录中，所以只写了文件名。

----------------

# 三、.pro.user文件
它包含了本地构建信息，包含Qt版本和构建目录等。可以用记事本或者写字板将这个文件打开查看其内容。当使用Qt Creator打开一个.pro文件时会自动生成一个.pro.user文件。因为读者的系统环境都不太一样，Qt的安装于设置也不尽相同，所以如果要将自己的源码公开，一般不需要包含这个user文件。如果要打开别人的项目文件，但里面包含了user文件，Qt Creator则会弹出提示窗口，询问是否载入特定的环境设置，这时应该选择“否”，然后选择自己的Qt版本即可。

# 四、生成文件说明
Qt Creator将项目源文件和编译生成的文件进行了分类存放。

helloworld文件夹中是项目源文件，还有两个文件夹是debug和release。

debug和release中的.o和.cpp文件都是过程文件，.exe是可执行文件。



参考自Qt开源社区的Qt学习之路,http://www.qter.org/forum.php?mod=viewthread&amp;tid=629。