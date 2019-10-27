---
title: QT-自定义信号和槽
date: 2019-1-17
categories:
- 笔记
tags:
- qt
toc: true
img: /medias/paperimg/qt.jpg
---
信号和槽是一种松耦合机制，或者说是一种分布式机制，信号广播出去，槽会自定义订阅接收。<!-- more -->
# 一、新建工程

# 二、新建部件

拖入button按钮。修改内容为“发送自定义信号”

![image](http://upload-images.jianshu.io/upload_images/16115686-2ee066fe7f985bcb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 三、自定义发送信号

## 3.1 修改widget.h文件

 添加处理按钮 clicked 信号的槽函数和新的自定义的信号 SendMsg。

```
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

signals:    //添加自定义的信号，信号强制为公有类型，所以没有前缀
    void SendMsg(QString str);  //信号只需要声明，不要给信号写实体代码

public slots:   //接收按钮信号的槽函数
    void ButtonClicked();

private:
    Ui::Widget *ui;
};

#endif // WIDGET_H
```

## 3.2 修改widget.cpp文件

```
#include "widget.h"
#include "ui_widget.h"

Widget::Widget(QWidget *parent) :
    QWidget(parent),
    ui(new Ui::Widget)
{
    ui->setupUi(this);

    //关联
    connect(ui->pushButton, SIGNAL(clicked()), this, SLOT(ButtonClicked()));
}

Widget::~Widget()
{
    delete ui;
}
//槽函数
void Widget::ButtonClicked()
{
    //用 emit 发信号
    emit SendMsg( tr("This is the message!") );
}
```

# 四、自定义接收槽

## 4.1 新建C++类

新建名字为“ShowMsg”的C++类，基类为QObject类。

## 4.2 修改 showmsg.h文件

声明接收 SendMsg 信号的槽函数 RecvMsg

```
#ifndef SHOWMSG_H
#define SHOWMSG_H

#include <QObject>

class ShowMsg : public QObject
{
    Q_OBJECT
public:
    explicit ShowMsg(QObject *parent = 0);
    ~ShowMsg();

signals:

public slots:
    //接收 SendMsg 信号的槽函数
    void RecvMsg(QString str);
};

#endif // SHOWMSG_H
```

## 4.3 修改showmsg.cpp文件

```
#include "showmsg.h"
#include <QMessageBox>

ShowMsg::ShowMsg(QObject *parent) : QObject(parent)
{

}

ShowMsg::~ShowMsg()
{

}

//str 就是从信号里发过来的字符串
void ShowMsg::RecvMsg(QString str)
{
    QMessageBox::information(NULL, tr("Show"), str);
}
```

## 4.4 修改main.cpp文件

```
#include "widget.h"
#include <QApplication>
#include "showmsg.h"

int main(int argc, char *argv[])
{
    QApplication a(argc, argv);
    Widget w;   //①主窗体对象，内部会发送 SendMsg 信号
    ShowMsg s;  //②接收端对象，有槽函数 RecvMsg
    //③关联，信号里的字符串参数会自动传递给槽函数
    QObject::connect(&w, SIGNAL(SendMsg(QString)), &s, SLOT(RecvMsg(QString)));

    //显示主界面
    w.show();

    return a.exec();
}
```
# 五、运行

![image](http://upload-images.jianshu.io/upload_images/16115686-46440774b31e602b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
