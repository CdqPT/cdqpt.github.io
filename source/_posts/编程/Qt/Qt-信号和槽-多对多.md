---
title: Qt-信号和槽-多对多
date: 2019-1-17
categories:
- 笔记
tags:
- qt
toc: true
img: /medias/paperimg/qt.jpg
---
介绍1对多，多对1以及多对多的案例。<!-- more -->
# 一、1对多

演示内容：在QLineEdit输入时，同步label，text browser以及调试输出板同步显示。

## 1.1 新建工程

## 1.2 添加部件

拖入line Edit、Label和Text Browser标签

![image](http://upload-images.jianshu.io/upload_images/16115686-45ea6d7ef8b69f29.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 1.3 修改文件

### 修改 widget.h 头文件

```c
namespace Ui {
class Widget;
}

class Widget : public QWidget
{
    Q_OBJECT

public:
    explicit Widget(QWidget *parent = 0);
    ~Widget();

public slots:   //添加槽函数打印调试信息
    void PrintText(const QString& text);

private:
    Ui::Widget *ui;
};
```

### 修改 widget.cpp 文件

```c
#include "widget.h"
#include "ui_widget.h"
#include <QDebug>  //qDebug函数需要的头文件

Widget::Widget(QWidget *parent) :
    QWidget(parent),
    ui(new Ui::Widget)
{
    ui->setupUi(this);

    connect(ui->lineEdit, SIGNAL(textEdited(QString)), ui->label, SLOT(setText(QString)));//将 lineEdit 的编辑信号关联到 label 的设置文本槽函数；
    connect(ui->lineEdit, SIGNAL(textEdited(QString)), ui->textBrowser, SLOT(setText(QString)));//将 lineEdit 的编辑信号关联到 textBrowser 的设置文本槽函数
    connect(ui->lineEdit, SIGNAL(textEdited(QString)), this, SLOT(PrintText(QString)));//将 lineEdit 的编辑信号关联到主窗体的 PrintText 槽函数
}

Widget::~Widget()
{
    delete ui;
}

void Widget::PrintText(const QString &text)
{
    qDebug()<<text; //打印到调试输出面板
}
```

## 1.4 运行

![image](http://upload-images.jianshu.io/upload_images/16115686-4ca63126c570b3df.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 二、多对1 

演示内容：三个信号对应一个槽。

## 2.1 新建项目

## 2.2 新建部件

拖入三个button，并将ObjectName分别修改为pushButtonA，pushButtonB和pushButtonC。

 ![image](http://upload-images.jianshu.io/upload_images/16115686-c8d0c5b8db30d606.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 2.3 修改文件

### 在widget.h中添加槽声明

```c
#ifndef WIDGET_H
#define WIDGET_H

#include <QWidget>

namespace Ui {
class Widget;
}

class Widget : public QWidget
{
    Q_OBJECT

public:
    explicit Widget(QWidget *parent = 0);
    ~Widget();

public slots:   //添加槽函数进行弹窗
    void FoodIsComing();

private:
    Ui::Widget *ui;
};

#endif // WIDGET_H
```


### 修改 widget.cpp 文件

```c
#include "widget.h"
#include "ui_widget.h"
#include <QMessageBox>
#include <QDebug>

Widget::Widget(QWidget *parent) :
    QWidget(parent),
    ui(new Ui::Widget)
{
    ui->setupUi(this);

    //三个按钮的信号都关联到 FoodIsComing 槽函数
    connect(ui->pushButtonA, SIGNAL(clicked()), this, SLOT(FoodIsComing()));
    connect(ui->pushButtonB, SIGNAL(clicked()), this, SLOT(FoodIsComing()));
    connect(ui->pushButtonC, SIGNAL(clicked()), this, SLOT(FoodIsComing()));
}

Widget::~Widget()
{
    delete ui;
}

void Widget::FoodIsComing()
{
    //获取信号源头对象的名称
    QString strObjectSrc = this->sender()->objectName();
    qDebug()<<strObjectSrc; //打印源头对象名称

    //将要显示的消息
    QString strMsg;
    //判断是哪个按钮发的信号
    if( "pushButtonA" == strObjectSrc )
    {
        strMsg = tr("Hello Anderson! Your food is coming!");
    }
    else if( "pushButtonB" == strObjectSrc )
    {
        strMsg = tr("Hello Bruce! Your food is coming!");
    }
    else if( "pushButtonC" == strObjectSrc )
    {
        strMsg = tr("Hello Castiel! Your food is coming!");
    }
    else
    {
        //do nothing
        return;
    }
    //显示送餐消息
    QMessageBox::information(this,tr("Food"),strMsg);
}
```

## 2.4 运行

![image](http://upload-images.jianshu.io/upload_images/16115686-705e9900b84e38bf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


参考自：https://qtguide.ustclug.org/
