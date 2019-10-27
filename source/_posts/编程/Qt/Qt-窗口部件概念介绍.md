---
title: Qt-窗口部件概念介绍
date: 2019-1-16
categories:
- 笔记
tags:
- qt
toc: true
img: /medias/paperimg/qt.jpg
---
包括基础窗口部件QWidget、对话框QDialog、QFrame类族 、按钮部件、行编辑器、数值设定框以及滑块部件。<!-- more -->
# 一、基础窗口部件QWidget

窗口部件（Widget）是Qt中建立用户界面的主要元素。像主窗口、对话框、标签、还有按钮、文本输入框等都是窗口部件。

QWidget类是所有用户界面对象的基类，被称为基础窗口部件。

在Qt中，把没有嵌入到其他部件中的部件称为窗口，一般的，窗口都有边框和标题栏。

QWidget的构造函数有两个参数：QWidget * parent = 0和Qt::WindowFlags f = 0；

前面的parent就是指父窗口部件，默认值为0，表明没有父窗口；后面的f参数是Qt::WindowFlags类型的，它是一个枚举类型，分为窗口类型（WindowType）和窗口标志（WindowFlags。前者可以定义窗口的类型，比如我们这里f=0，表明使用了Qt::Widget一项，这是QWidget的默认类型，这种类型的部件如果有父窗口，那么它就是子部件，否则就是独立的窗口。

# 二、对话框Dialog

## 2.1 模态对话框

模态对话框就是在我们没有关闭它之前，不能再与同一个应用程序的其他窗口进行交互，比如新建项目时弹出的对话框。要想使一个对话框成为模态对话框，只需要调用它的exec()函数：

```
QDialog dialog(this);
dialog.exec();
```

或

```
QDialog *dialog = new QDialog(this);
dialog->setModal(true);
dialog->show();
```

## 2.2 非模态对话框

非模态对话框，既可以与它交互，也可以与同一程序中的其他窗口交互，例如Microsoft Word中的查找替换对话框。要使一个对话框成为非模态对话框，我们就可以使用new操作来创建，然后使用show()函数来显示。

```
QDialog *dialog = new QDialog(this);
dialog->show();
```

# 2.3 对话框类型

*   颜色对话框
*   文件对话框
*   字体对话框
*   输入对话框
*   消息对话框
*   进度对话框
*   错误信息对话框
*   向导对话框

例如：

QColor color = QColorDialog::getColor(Qt::red, this, tr("颜色对话框"));

![image](http://upload-images.jianshu.io/upload_images/16115686-3a5bafcfbb0cbeb7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 三、QFrame类族 

 QFrame类是带有边框的部件的基类。

它的子类有我们最为常用的标签部件QLabel、QLCDNumber、QSplitter、QStackedWidget、QToolBox和QAbstractScrollArea类。

 ![image](http://upload-images.jianshu.io/upload_images/16115686-df91a92ba7dcc7ed.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 四、按钮部件

QAbstractButton类是按钮部件的抽象基类，提供了按钮的通用功能。它的子类包括：

复选框QCheckBox 、标准按钮QPushButton 、单选框按钮QRadioButton和工具按钮QToolButton。

# 五、行编辑器

行编辑器QLineEdit部件是一个单行的文本编辑器，它允许用户输入和编辑单行的纯文本内容，而且提供了一系列有用的功能，包括撤销与恢复、剪切和拖放等操作。 例如：

显示模式 、输入掩码、 输入验证 和自动补全

# 六、数值设定框

QAbstractSpinBox类是一个抽象基类，它提供了一个数值设定框和一个行编辑器来显示设定值。它有三个子类：

QDateTimeEdit（日期时间设定） 、QSpinBox（整数设定） 、QDoubleSpinBox（浮点数的设定）

# 七、滑块部件

QAbstractSlider类提供了一个区间内的整数值，它有一个滑块，可以定位到一个整数区间的任意值。这个类是一个抽象基类，它有三个子类QScrollBar，QSlider和QDial。其中：

滚动条QScrollBar更多的是用在QScrollArea类中来实现滚动区域；

QSlider是我们最常见的音量控制或多媒体播放进度等滑块；

QDial是一个刻度表盘。


参考自Qt开源社区的Qt学习之路,http://www.qter.org/forum.php?mod=viewthread&amp;tid=629。
