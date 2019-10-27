---
title: QT-布局管理
date: 2019-1-16
categories:
- 笔记
tags:
- qt
toc: true
img: /medias/paperimg/qt.jpg
---
Qt的布局管理系统提供了简单而强大的机制来自动排列一个窗口中的部件，确保它们有效的使用空间。<!-- more -->
# 一、布局管理器
## 1.1 QLayout
QLayout类是布局管理器的基类，它是一个抽象基类。一般只需要使用QLayout的几个子类就可以了，它们分别是：

* QBoxLayout（基本布局管理器）
* QGridLayout（栅格布局管理器）
* QFormLayout（窗体布局管理器）
* QStackedLayout（栈布局管理器）
## 1.2 基本布局管理器QBoxLayout
基本布局管理器QBoxLayout类可以使子部件在水平方向或者垂直方向排成一列，它将所有的空间分成一行盒子，然后将每个部件放入一个盒子中。

它有两个子类QHBoxLayout水平布局管理器和QVBoxLayout垂直布局管理器。布局管理器的几个属性如下表所示：

|属性|	说明|
|:---:|:---:|
|layoutName	|现在所使用的布局管理器的名称|
|layoutLeftMargin|	设置布局管理器到界面左边界的距离|
|layoutTopMargin|	设置布局管理器到界面上边界的距离|
|layoutRightMargin	|设置布局管理器到界面右边界的距离|
|layoutBottomMargin	|设置布局管理器到界面下边界的距离|
|layoutSpacing|	布局管理器中各个子部件间的距离|
|layoutStretch	|伸缩因子|
|layoutSizeConstraint	|设置的大小约束条件|
## 1.3 使用代码实现水平布局
```
QHBoxLayout *layout = new QHBoxLayout;      // 新建水平布局管理器
layout->addWidget(ui->fontComboBox);        // 向布局管理器中添加部件
layout->addWidget(ui->textEdit);
layout->setSpacing(50);                     // 设置部件间的间隔
layout->setContentsMargins(0, 0, 50, 100); // 设置布局管理器到边界的距离，四个参数顺序是左，上，右，下
setLayout(layout); 
```
## 1.4 栅格布局管理器QGridLayout
栅格布局管理器QGridLayout类使得部件在网格中进行布局，它将所有的空间分隔成一些行和列，行和列的交叉处就形成了单元格，然后将部件放入一个确定的单元格中。例如：
```
QGridLayout *layout = new QGridLayout;// 添加部件，从第0行0列开始，占据1行2列
layout->addWidget(ui->fontComboBox, 0, 0, 1, 2);// 添加部件，从第0行2列开始，占据1行1列
layout->addWidget(ui->pushButton, 0, 2, 1, 1);// 添加部件，从第1行0列开始，占据1行3列
layout->addWidget(ui->textEdit, 1, 0, 1, 3);
setLayout(layout);
```
说明：当部件加入到一个布局管理器中，然后这个布局管理器再放到一个窗口部件上时，这个布局管理器以及它包含的所有部件都会自动重新定义自己的父对象（parent）为这个窗口部件，所以在创建布局管理器和其中的部件时并不用指定父部件。

1.5 窗体布局管理器QFormLayout
窗体布局管理器QFormLayout类用来管理表单的输入部件和与它们相关的标签。窗体布局管理器将它的子部件分为两列，左边是一些标签，右边是一些输入部件，比如行编辑器或者数字选择框等。

 


参考自Qt开源社区的Qt学习之路,http://www.qter.org/forum.php?mod=viewthread&amp;tid=629。