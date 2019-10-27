---
title: Qt学习笔记
author: 陈德强
date: 2019-03-19 10:37:00
categories:
- 笔记
tags:
- qt
toc: true
top: false
img: /medias/paperimg/qt.jpg
summary:  Qt基础笔记。
---

# 一、信号和槽

connect(sender, signal, receiver, slot)函数有四个参数，分别是发送者，信号（发送方式），接收者，槽函数。
## 1.1 内置信号和槽
```c
 QObject::connect(&button, &QPushButton::clicked, [](bool) {qDebug() << "You clicked me!"; });
```
这里用到了lambda表达式。

## 1.2 自定义信号和槽
newspaper.h
```c
//!!! Qt5
#include <QObject>
 

class Newspaper : public QObject
{
    Q_OBJECT
public:
    Newspaper(const QString & name) :
        m_name(name)
    {
    }
 
    void send()
    {
        emit newPaper(m_name);
    }
 
signals:
    void newPaper(const QString &name);
 
private:
    QString m_name;
};
``` 
 reader.h
```c
#include <QObject>
#include <QDebug>
 
class Reader : public QObject
{
    Q_OBJECT
public:
    Reader() {}
 
    void receiveNewspaper(const QString & name)
    {
        qDebug() << "Receives Newspaper: " << name;
    }
};
``` 
 main.cpp
```c
#include <QCoreApplication>
 
#include "newspaper.h"
#include "reader.h"
 
int main(int argc, char *argv[])
{
    QCoreApplication app(argc, argv);
 
    Newspaper newspaper("Newspaper A");
    Reader reader;
    QObject::connect(&newspaper, &Newspaper::newPaper,
                     &reader,    &Reader::receiveNewspaper);
    newspaper.send();
 
    return app.exec();
}
```

# 二、MainWindown使用

添加动作：

mainwindow.h

```c
#ifndef MAINWINDOW_H
#define MAINWINDOW_H

#include <QMainWindow>

class MainWindow : public QMainWindow
{
    Q_OBJECT
public:
    MainWindow(QWidget *parent = 0);
    ~MainWindow();

private:
    void open();

    QAction *openAction;
};

#endif // MAINWINDOW_H

```

mainwindow.cpp

```c
#include <QAction>
#include <QMenuBar>
#include <QMessageBox>
#include <QStatusBar>
#include <QToolBar>

#include "mainwindow.h"

MainWindow::MainWindow(QWidget *parent) :
    QMainWindow(parent)
{
    setWindowTitle(tr("主菜单"));//设置主菜单名称

    openAction = new QAction(QIcon("C://Users//Administrator//Documents//untitled1//Open-Folder.png"),
                             tr("&Open..."), this);//设置动作图标及名称
    openAction->setShortcuts(QKeySequence::Open);//设置快捷键
    openAction->setStatusTip(tr("Open an existing file"));//设置鼠标滑过显示的信息
    connect(openAction, &QAction::triggered, this, &MainWindow::open);//当有操作时执行open窗口函数

    QMenu *file = menuBar()->addMenu(tr("&File"));//添加到菜单栏，显示为菜单项
    file->addAction(openAction);

    QToolBar *toolBar = addToolBar(tr("&File"));//添加到导航栏，显示为按钮
    toolBar->addAction(openAction);

    statusBar() ;//启用状态栏，显示setStatusTip中的信息
}

MainWindow::~MainWindow()
{
}

void MainWindow::open()
{
    QMessageBox::information(this, tr("Information"), tr("Open"));
}

```
效果如下：
![image](https://ws3.sinaimg.cn/large/006jQClrly1g1813admr4j30ii099a9z.jpg)

# 三、资源文件

资源文件用来存放图片，视频等资源的。

有两点需要注意：

1. 文件路径，必须全英文。

2. 不要摁左上角的新建文件，这个选了QRC(资源文件)不能挂到项目里。对着项目菜单里 的文件夹右键新建QRC。
## 3.1 新建资源文件管理
名称为image。

![3-1](https://ws3.sinaimg.cn/large/006jQClrly1g181ri3gw2j31hc0u0wmd.jpg)
![3-2](https://ws2.sinaimg.cn/large/006jQClrly1g181tdb3zqj30o90f174v.jpg)

## 3.2 添加资源文件

点击添加，前缀名称icon，在点击添加，添加文件，选择文件，点击图片，添加别名。
![3-3](https://ws2.sinaimg.cn/large/006jQClrly1g181vksgvvj31fu0u0jtw.jpg)

这时，第二章中的

```
 openAction = new QAction(QIcon("C://Users//Administrator//Documents//untitled1//Open-Folder.png"),
```

就可以替换为

```
 openAction = new QAction(QIcon(":/icon/open"),
```
# 四、布局管理器

## 4.1 自动调整

```c

#include "mainwindow.h"
#include <QApplication>
#include <QSpinBox>
#include <QSlider>
#include <QHBoxLayout>
int main(int argc, char *argv[])
{
    QApplication app(argc, argv);

    QWidget window;
    window.setWindowTitle("Enter your age");

    QSpinBox *spinBox = new QSpinBox(&window);
    QSlider *slider = new QSlider(Qt::Horizontal, &window);
    spinBox->setRange(0, 130);//带有上下箭头的数字输入框
    slider->setRange(0, 130);//带有滑块的滑竿

    QObject::connect(slider, &QSlider::valueChanged, spinBox, &QSpinBox::setValue);
    void (QSpinBox:: *spinBoxSignal)(int) = &QSpinBox::valueChanged;
    QObject::connect(spinBox, spinBoxSignal, slider, &QSlider::setValue);
    spinBox->setValue(35);

    QHBoxLayout *layout = new QHBoxLayout;
    layout->addWidget(spinBox);
    layout->addWidget(slider);
    window.setLayout(layout);

    window.show();

    return app.exec();
}
```

效果如下：

![image](https://wx1.sinaimg.cn/large/006jQClrly1g185dumjzoj30b4043jr7.jpg)

# 五、对话框（dialog）
## 5.1对话框的使用 
**没设置parent指针：**

  ```c
#include <QAction>
#include <QMenuBar>
#include <QMessageBox>
#include <QStatusBar>
#include <QToolBar>

#include "mainwindow.h"

MainWindow::MainWindow(QWidget *parent) :
    QMainWindow(parent)
{
    setWindowTitle(tr("主菜单"));//设置主菜单名称

    openAction = new QAction(QIcon("C://Users//Administrator//Documents//untitled1//Open-Folder.png"),
                             tr("&Open..."), this);//设置动作图标及名称
    openAction->setShortcuts(QKeySequence::Open);//设置快捷键
    openAction->setStatusTip(tr("Open an existing file"));//设置鼠标滑过显示的信息
    connect(openAction, &QAction::triggered, this, &MainWindow::open);//当有操作时执行open窗口函数

    QMenu *file = menuBar()->addMenu(tr("&File"));//添加到菜单栏，显示为菜单项
    file->addAction(openAction);

    QToolBar *toolBar = addToolBar(tr("&File"));//添加到导航栏，显示为按钮
    toolBar->addAction(openAction);

    statusBar() ;//启用状态栏，显示setStatusTip中的信息
}

MainWindow::~MainWindow()
{
}

void MainWindow::open()
{
    QDialog dialog;
    dialog.setWindowTitle(tr("Hello, dialog!"));
    dialog.exec();
}
```
显示如下：
![image](https://ws3.sinaimg.cn/large/006jQClrly1g188tbjuisj30o60et76a.jpg)

**设置parent指针：**

修改一下 open() 函数的内容：
```
void MainWindow::open()
{
    QDialog dialog(this);
    dialog.setWindowTitle(tr("Hello, dialog!"));
    dialog.exec();
}
```
显示如下：

![image](https://ws2.sinaimg.cn/large/006jQClrly1g188yhf7wuj30cp083t8o.jpg)

## 5.2 标准对话框QMessageBox
### 5.2.1 询问对话框
续5.1...

修改open() 函数内容：
```c
if (QMessageBox::Yes == QMessageBox::question(this,
                                              tr("Question"),
                                              tr("Are you OK?"),
                                              QMessageBox::Yes |QMessageBox::No,
                                              QMessageBox::Yes))//这里是把YES判断放在前边写了，正常放在后边的。
 {
QMessageBox::information(this, tr("Hmmm..."), tr("I'm glad to hear that!"));
} else {
    QMessageBox::information(this, tr("Hmmm..."), tr("I'm sorry!"));
}
```
QMessageBox::question() 是询问对话框，对一个参数是指定父窗口，第二个参数是对话框标题，第三个参数是显示内容，第四个参数是要显示的按钮，最后一个参数指定默认选择的按钮。
其中，


效果如下：
![image](https://ws2.sinaimg.cn/large/006jQClrly1g18x4u5h87j30ea08qjri.jpg)
### 5.2.2 保存对话框

修改open（）函数
```c
QMessageBox msgBox;
msgBox.setText(tr("The document has been modified."));
msgBox.setInformativeText(tr("Do you want to save your changes?"));
msgBox.setDetailedText(tr("Differences here..."));
msgBox.setStandardButtons(QMessageBox::Save
                          | QMessageBox::Discard
                          | QMessageBox::Cancel);
msgBox.setDefaultButton(QMessageBox::Save);
int ret = msgBox.exec();
switch (ret) {
case QMessageBox::Save:
    qDebug() << "Save document!";
    break;
case QMessageBox::Discard:
    qDebug() << "Discard changes!";
    break;
case QMessageBox::Cancel:
    qDebug() << "Close document!";
    break;
}
```
显示如下：
![image](https://wx3.sinaimg.cn/large/006jQClrly1g18xjxx42rj30a707daa3.jpg)
### 5.2.3 文件对话框