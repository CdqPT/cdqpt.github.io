---
title: QT-解除connect
date: 2019-1-17
categories:
- 笔记
tags:
- qt
toc: true
img: /medias/paperimg/qt.jpg
---
解除关联。<!-- more -->
# 一、新建工程

# 二、新建部件

在ui设计界面拖入一个line edit，一个label以及两个button按钮

![image](http://upload-images.jianshu.io/upload_images/16115686-0c732e2b569fa4f6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

右键“关联”按钮转到槽，选择clicked()，添加如下代码：

```
void Widget::on_pushButton_clicked()
{
    //关联
    connect(ui->lineEdit, SIGNAL(textEdited(QString)), ui->label, SLOT(setText(QString)));

    //调整按钮可用性
    ui->pushButton->setEnabled(false);      //已经关联，禁用关联按钮
    ui->pushButton_2->setEnabled(true);    //已经关联，只有解除关联按钮可用
}
```

右键“解除关联”按钮转到槽，选择clicked()，添加如下代码：

```
void Widget::on_pushButton_2_clicked()
{
    //解除关联
       disconnect(ui->lineEdit, SIGNAL(textEdited(QString)), ui->label, SLOT(setText(QString)));

       //调整按钮可用性
       ui->pushButton->setEnabled(true);       //没有关联，只有关联按钮可用
       ui->pushButton_2->setEnabled(false);   //没有关联，解除关联按钮禁用
}
```

# 三、运行

![image](http://upload-images.jianshu.io/upload_images/16115686-3c7977df10bbf058.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
