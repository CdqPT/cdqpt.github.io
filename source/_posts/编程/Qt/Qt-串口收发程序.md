---
title: Qt-串口收发程序
author: 陈德强
date: 2019-03-20 8:30:00
categories:
- 笔记
tags:
- qt
toc: true
top: false
img: /medias/paperimg/qt.jpg
summary:  串口收发程序。
---

# 一、实例

## 1.1 添加串口组件

在.pro文件中添加串口组件
```
QT       += core gui
QT       += serialport
```

## 1.2 UI文件

所应用到的组件有：
label，标签组件；
text edit，文本编辑组件；
pushputton，按钮组件；
combobox，下拉列表框；
vertical space，垂直空间；
vertical laout，垂直布局；
horizontal layout，水平布局。

命名列表：
不包括label命名。

|部件|一版命名|二版命名|
|:-------:|:-------:|:-------:|
|接收窗口|textEdit|recvTextEdit|
|发送窗口|textEdit_2|sendTextEdit|
|清空接收|clearButton|clearButton|
|发送数据|sendButton|sendButton|
|串口号|PortBox|portNameBox|
|波特率|BaudBox|baudrateBox|
|数据位|BitNumBox|dataBitsBox|
|校验位|ParityBox|ParityBox|
|停止位|StopBox|stopBitsBox|
|检测串口|--|searchButton|
|打开串口|openButton|openButton|


![布局图](https://wx1.sinaimg.cn/large/006jQClrly1g29e8amv94j30cu0aat8r.jpg)

## 1.3 修改mainwindow.h文件

添加头文件：
```c
#include <QSerialPort>
#include <QSerialPortInfo>
#include <QDebug>
```

定义串口私有变量：
```c++
private:
    Ui::MainWindow *ui;
    QSerialPort *serial;
```

## 1.4 编辑组件内容

清空接收按钮，提升为单击槽：
```c
//清空接受窗口
void MainWindow::on_clearButton_clicked()
{
    ui->textEdit->clear();
}
```
发送数据按钮，提升为单击槽：
```c
//发送数据
void MainWindow::on_sendButton_clicked()
{
    //toPlainText()和toHtml()方法能够得到它text形式的内容和html形式的内容。
    //toLatin1是一种汉字编码。
    serial->write(ui->textEdit_2->toPlainText().toLatin1());
}
```

打开串口按钮，提升为单击槽：
```c
void MainWindow::on_openButton_clicked()
{
    //分为打开和关闭两种情况
    if(ui->openButton->text()==tr("打开串口"))
    {
        serial = new QSerialPort;
        //设置串口名
        serial->setPortName(ui->PortBox->currentText());
        //打开串口,开启串口读写功能。
        serial->open(QIODevice::ReadWrite);
        //设置波特率
        serial->setBaudRate(ui->BaudBox->currentText().toInt());
        //设置数据位数
        switch(ui->BitNumBox->currentIndex())
        {
        case 8: serial->setDataBits(QSerialPort::Data8); break;
        default: break;
        }
        //设置奇偶校验
        switch(ui->ParityBox->currentIndex())
        {
        case 0: serial->setParity(QSerialPort::NoParity); break;
        default: break;
        }
        //设置停止位
        switch(ui->StopBox->currentIndex())
        {
        case 1: serial->setStopBits(QSerialPort::OneStop); break;
        case 2: serial->setStopBits(QSerialPort::TwoStop); break;
        default: break;
        }
        //设置流控制
        serial->setFlowControl(QSerialPort::NoFlowControl);

        //关闭设置菜单使能
        ui->PortBox->setEnabled(false);
        ui->BaudBox->setEnabled(false);
        ui->BitNumBox->setEnabled(false);
        ui->ParityBox->setEnabled(false);
        ui->StopBox->setEnabled(false);
        ui->openButton->setText(tr("关闭串口"));
        ui->sendButton->setEnabled(true);

        //连接信号槽，当串口读取数据时，窗口调用Read_Data函数。
        QObject::connect(serial, &QSerialPort::readyRead, this, &MainWindow::Read_Data);
    }
    else
    {
        //关闭串口
        serial->clear();
        serial->close();
        serial->deleteLater();

        //恢复设置使能
        ui->PortBox->setEnabled(true);
        ui->BaudBox->setEnabled(true);
        ui->BitNumBox->setEnabled(true);
        ui->ParityBox->setEnabled(true);
        ui->StopBox->setEnabled(true);
        ui->openButton->setText(tr("打开串口"));
        ui->sendButton->setEnabled(false);
    }
}
```

在mainwindow.h文件中添加Read_Data定义：
```
private slots:
    void on_clearButton_clicked();

    void on_sendButton_clicked();

    void on_openButton_clicked();

    void Read_Data();
```
在mainwindow.cpp文件中新建槽函数Read_Data，放在void MainWindow::on_openButton_clicked()前面：
```c
//读取接收到的数据
void MainWindow::Read_Data()
{
    //从接收缓冲区中读取数据
    QByteArray buf = serial->readAll();

    if(!buf.isEmpty())
    {
        //从界面中读取以前收到的数据
        QString str = ui->textEdit->toPlainText();
        str+=tr(buf);
        //清空以前的显示
        ui->textEdit->clear();
        //重新显示
        ui->textEdit->append(str);
    }
    buf.clear();
}
```
修改mainwindow.cpp的初始化函数：
```c
MainWindow::MainWindow(QWidget *parent) :
    QMainWindow(parent),
    ui(new Ui::MainWindow)
{
    ui->setupUi(this);

    //查找可用的串口
    foreach(const QSerialPortInfo &info, QSerialPortInfo::availablePorts())
    {
        QSerialPort serial;
        serial.setPort(info);
        if(serial.open(QIODevice::ReadWrite))
        {
            ui->PortBox->addItem(serial.portName());
            serial.close();
        }
    }
    //设置波特率下拉菜单默认显示第三项
    ui->BaudBox->setCurrentIndex(3);
    //关闭发送按钮的使能
    ui->sendButton->setEnabled(false);
    qDebug() << tr("界面设定成功！");
}
```
## 1.5 arduino测试程序

```c
void setup()
{
  Serial.begin(9600);
  pinMode(13,OUTPUT);
  }
void loop()
{
    Serial.write("123");
    delay(500);
}
```



# 二、源码

## 2.1 .pro文件
```c
#-------------------------------------------------
#
# Project created by QtCreator 2017-11-17T15:43:04
#
#-------------------------------------------------

QT       += core gui
QT       += serialport

greaterThan(QT_MAJOR_VERSION, 4): QT += widgets

TARGET = SerialPortTool
TEMPLATE = app

# The following define makes your compiler emit warnings if you use
# any feature of Qt which as been marked as deprecated (the exact warnings
# depend on your compiler). Please consult the documentation of the
# deprecated API in order to know how to port your code away from it.
DEFINES += QT_DEPRECATED_WARNINGS

# You can also make your code fail to compile if you use deprecated APIs.
# In order to do so, uncomment the following line.
# You can also select to disable deprecated APIs only up to a certain version of Qt.
#DEFINES += QT_DISABLE_DEPRECATED_BEFORE=0x060000    # disables all the APIs deprecated before Qt 6.0.0


SOURCES += \
        main.cpp \
        mainwindow.cpp

HEADERS += \
        mainwindow.h

FORMS += \
        mainwindow.ui
```
## 2.2 mainwindow.h文件
```c
#ifndef MAINWINDOW_H
#define MAINWINDOW_H

#include <QMainWindow>
#include <QDebug>
#include <QtSerialPort/QSerialPort>
#include <QtSerialPort/QSerialPortInfo>

namespace Ui {
class MainWindow;
}

class MainWindow : public QMainWindow
{
    Q_OBJECT

public:
    explicit MainWindow(QWidget *parent = 0);
    ~MainWindow();

private slots:
    void on_OpenSerialButton_clicked();

    void ReadData();

    void on_SendButton_clicked();

private:
    Ui::MainWindow *ui;
    QSerialPort *serial;
};

#endif // MAINWINDOW_H
```
## 2.3 mainwindow.cpp文件
```c
#include "mainwindow.h"
#include "ui_mainwindow.h"


MainWindow::MainWindow(QWidget *parent) :
    QMainWindow(parent),
    ui(new Ui::MainWindow)
{
    ui->setupUi(this);

    //查找可用的串口
    foreach (const QSerialPortInfo &info,QSerialPortInfo::availablePorts())
    {
        QSerialPort serial;
        serial.setPort(info);
        if(serial.open(QIODevice::ReadWrite))
        {
            ui->PortBox->addItem(serial.portName());
            serial.close();
        }
    }
    //设置波特率下拉菜单默认显示第0项
    ui->BaudBox->setCurrentIndex(0);

}

MainWindow::~MainWindow()
{
    delete ui;
}

void MainWindow::on_OpenSerialButton_clicked()
{
    if(ui->OpenSerialButton->text() == tr("打开串口"))
    {
        serial = new QSerialPort;
        //设置串口名
        serial->setPortName(ui->PortBox->currentText());
        //打开串口
        serial->open(QIODevice::ReadWrite);
        //设置波特率
        serial->setBaudRate(QSerialPort::Baud115200);//设置波特率为115200
        //设置数据位数
        switch (ui->BitBox->currentIndex())
        {
        case 8:
            serial->setDataBits(QSerialPort::Data8);//设置数据位8
            break;
        default:
            break;
        }
        //设置校验位
        switch (ui->ParityBox->currentIndex())
        {
        case 0:
            serial->setParity(QSerialPort::NoParity);
            break;
        default:
            break;
        }
        //设置停止位
        switch (ui->BitBox->currentIndex())
        {
        case 1:
            serial->setStopBits(QSerialPort::OneStop);//停止位设置为1
            break;
        case 2:
            serial->setStopBits(QSerialPort::TwoStop);
        default:
            break;
        }
        //设置流控制
        serial->setFlowControl(QSerialPort::NoFlowControl);//设置为无流控制

        //关闭设置菜单使能
        ui->PortBox->setEnabled(false);
        ui->BaudBox->setEnabled(false);
        ui->BitBox->setEnabled(false);
        ui->ParityBox->setEnabled(false);
        ui->StopBox->setEnabled(false);
        ui->OpenSerialButton->setText(tr("关闭串口"));

        //连接信号槽
        QObject::connect(serial,&QSerialPort::readyRead,this,&MainWindow::ReadData);
    }
    else
    {
        //关闭串口
        serial->clear();
        serial->close();
        serial->deleteLater();

        //恢复设置使能
        ui->PortBox->setEnabled(true);
        ui->BaudBox->setEnabled(true);
        ui->BitBox->setEnabled(true);
        ui->ParityBox->setEnabled(true);
        ui->StopBox->setEnabled(true);
        ui->OpenSerialButton->setText(tr("打开串口"));

    }




}
//读取接收到的信息
void MainWindow::ReadData()
{
    QByteArray buf;
    buf = serial->readAll();
    if(!buf.isEmpty())
    {
        QString str = ui->textEdit->toPlainText();
        str+=tr(buf);
        ui->textEdit->clear();
        ui->textEdit->append(str);

    }
    buf.clear();
}

//发送按钮槽函数
void MainWindow::on_SendButton_clicked()
{
    serial->write(ui->textEdit_2->toPlainText().toLatin1());
}
```
## 2.4 mainwindow.ui文件


```c
<?xml version="1.0" encoding="UTF-8"?>
<ui version="4.0">
 <class>MainWindow</class>
 <widget class="QMainWindow" name="MainWindow">
  <property name="geometry">
   <rect>
    <x>0</x>
    <y>0</y>
    <width>547</width>
    <height>470</height>
   </rect>
  </property>
  <property name="windowTitle">
   <string>MainWindow</string>
  </property>
  <widget class="QWidget" name="centralWidget">
   <widget class="QLabel" name="label">
    <property name="geometry">
     <rect>
      <x>10</x>
      <y>50</y>
      <width>54</width>
      <height>12</height>
     </rect>
    </property>
    <property name="text">
     <string>串口</string>
    </property>
   </widget>
   <widget class="QLabel" name="label_2">
    <property name="geometry">
     <rect>
      <x>10</x>
      <y>90</y>
      <width>54</width>
      <height>12</height>
     </rect>
    </property>
    <property name="text">
     <string>波特率</string>
    </property>
   </widget>
   <widget class="QLabel" name="label_3">
    <property name="geometry">
     <rect>
      <x>10</x>
      <y>130</y>
      <width>54</width>
      <height>12</height>
     </rect>
    </property>
    <property name="text">
     <string>数据位</string>
    </property>
   </widget>
   <widget class="QComboBox" name="PortBox">
    <property name="geometry">
     <rect>
      <x>100</x>
      <y>50</y>
      <width>69</width>
      <height>22</height>
     </rect>
    </property>
   </widget>
   <widget class="QComboBox" name="BaudBox">
    <property name="geometry">
     <rect>
      <x>100</x>
      <y>90</y>
      <width>69</width>
      <height>22</height>
     </rect>
    </property>
    <property name="currentIndex">
     <number>0</number>
    </property>
    <item>
     <property name="text">
      <string>9600</string>
     </property>
    </item>
    <item>
     <property name="text">
      <string>19200</string>
     </property>
    </item>
    <item>
     <property name="text">
      <string>38400</string>
     </property>
    </item>
    <item>
     <property name="text">
      <string>57600</string>
     </property>
    </item>
    <item>
     <property name="text">
      <string>115200</string>
     </property>
    </item>
   </widget>
   <widget class="QComboBox" name="BitBox">
    <property name="geometry">
     <rect>
      <x>100</x>
      <y>120</y>
      <width>69</width>
      <height>22</height>
     </rect>
    </property>
    <property name="currentIndex">
     <number>0</number>
    </property>
    <item>
     <property name="text">
      <string>8</string>
     </property>
    </item>
   </widget>
   <widget class="QComboBox" name="ParityBox">
    <property name="geometry">
     <rect>
      <x>100</x>
      <y>160</y>
      <width>69</width>
      <height>22</height>
     </rect>
    </property>
    <item>
     <property name="text">
      <string>0</string>
     </property>
    </item>
   </widget>
   <widget class="QLabel" name="label_4">
    <property name="geometry">
     <rect>
      <x>10</x>
      <y>160</y>
      <width>61</width>
      <height>16</height>
     </rect>
    </property>
    <property name="text">
     <string>校验位</string>
    </property>
   </widget>
   <widget class="QLabel" name="label_6">
    <property name="geometry">
     <rect>
      <x>10</x>
      <y>200</y>
      <width>54</width>
      <height>12</height>
     </rect>
    </property>
    <property name="text">
     <string>停止位</string>
    </property>
   </widget>
   <widget class="QComboBox" name="StopBox">
    <property name="geometry">
     <rect>
      <x>100</x>
      <y>200</y>
      <width>69</width>
      <height>22</height>
     </rect>
    </property>
    <item>
     <property name="text">
      <string>1</string>
     </property>
    </item>
   </widget>
   <widget class="QPushButton" name="OpenSerialButton">
    <property name="geometry">
     <rect>
      <x>100</x>
      <y>240</y>
      <width>71</width>
      <height>23</height>
     </rect>
    </property>
    <property name="text">
     <string>打开串口</string>
    </property>
   </widget>
   <widget class="QTextEdit" name="textEdit">
    <property name="geometry">
     <rect>
      <x>200</x>
      <y>30</y>
      <width>221</width>
      <height>291</height>
     </rect>
    </property>
   </widget>
   <widget class="QTextEdit" name="textEdit_2">
    <property name="geometry">
     <rect>
      <x>200</x>
      <y>330</y>
      <width>221</width>
      <height>31</height>
     </rect>
    </property>
   </widget>
   <widget class="QPushButton" name="SendButton">
    <property name="geometry">
     <rect>
      <x>430</x>
      <y>330</y>
      <width>75</width>
      <height>31</height>
     </rect>
    </property>
    <property name="text">
     <string>发送</string>
    </property>
   </widget>
  </widget>
  <widget class="QMenuBar" name="menuBar">
   <property name="geometry">
    <rect>
     <x>0</x>
     <y>0</y>
     <width>547</width>
     <height>23</height>
    </rect>
   </property>
  </widget>
  <widget class="QToolBar" name="mainToolBar">
   <attribute name="toolBarArea">
    <enum>TopToolBarArea</enum>
   </attribute>
   <attribute name="toolBarBreak">
    <bool>false</bool>
   </attribute>
  </widget>
  <widget class="QStatusBar" name="statusBar"/>
 </widget>
 <layoutdefault spacing="6" margin="11"/>
 <resources/>
 <connections/>
</ui>
```


## 2.5 arduino测试程序

```c
void setup()
{
  Serial.begin(9600);
  pinMode(13,OUTPUT);
  }
void loop()
{
    Serial.write("123");
    delay(500);
}
```

## 2.6 效果图

![image](https://wx1.sinaimg.cn/large/006jQClrly1g2a1rfpv02j30mg0hnjro.jpg)



参考文献：
[QT5串口编程——编写简单的上位机](https://blog.csdn.net/u014695839/article/details/50611549)