---
title: QT-helloworld-Qt设计师编写
date: 2019-1-16
categories:
- 笔记
tags:
- qt
toc: true
img: /medias/paperimg/qt.jpg
---
Qt设计师界面类就是C++类和ui文件的结合，它将这两个文件一起生成了，而不用再逐一添加。<!-- more -->
目标：在对话框中显示出“helloworld”字样。

# 一、新建项目

## 1.1 选择项目模板

```
文件→新建文件或项目→Application→Qt Widgets Application→Choose
```

Qt Widgets Application就是Qt设计师

## 1.2 输入项目信息

```
项目介绍和位置→名称→helloworld→自定义路径（不要有中文）→下一步→下一步
```
## 1.3 输入类信息

这步输入自定义类名和基类信息。自定义类会继承基类的全部属性。换句话说基类就是模版，我们在其上修改成自己需要的功能。

```
类名→HelloDialog→基类→QDialog→下一步→下一步
```
# 二、设计ui界面

##  2.1 双击.ui文件，进入设计模式。

![image](http://upload-images.jianshu.io/upload_images/16115686-44a3e0be4b93b5c9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 2.2 设计ui

按着鼠标左键拖动label到主设计区，双击输入helloworld！。

![image](http://upload-images.jianshu.io/upload_images/16115686-169a1eb7c57c1474.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 三、设置图标

## 3.1 下载图标

可从图标网[https://www.easyicon.net/](https://www.easyicon.net/)下载图标，例如我搜索了ball，然后将下载的.ico图标复制到工程文件夹的helloworld文件夹中并重命名为ball.ico

## 3.2 修改项目文件

打开helloworld.pro文件，在最后面添加下面一行代码：

```
RC_ICONS = ball.ico
```

# 四、运行与发布

## 4.1 运行程序

**按左下角三角图标运行程序**

 ![image](http://upload-images.jianshu.io/upload_images/16115686-fec5e387ba7607db.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**设置环境变量**

将 C:\Qt\Qt5.6.1\5.6\mingw49_32\bin 添加到path中，配置环境变量可参考：[https://jingyan.baidu.com/article/00a07f3876cd0582d128dc55.html](https://jingyan.baidu.com/article/00a07f3876cd0582d128dc55.html)

注：红色部分根据自己的目录进行对应修改。

这样就可以运行.exe程序了。

## 4.2 发布程序

将构建目标设置为Release。

 ![image](http://upload-images.jianshu.io/upload_images/16115686-74012d90c82040be.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

编译。

新建“我的第一个Qt程序”文件夹，然后将release文件夹中的helloworld.exe复制过来，再去Qt安装目录的bin目录中将

libgcc_s_dw2-1.dll、libstdc++-6.dll、libwinpthread-1.dll、Qt5Core.dll、Qt5Gui.dll和Qt5Widgets.dll这6个文件复制过来 。

将C:\Qt\Qt5.6.1\5.6\mingw49_32\plugins目录中的platforms文件夹复制过来（不要修改该文件夹名称），里面只需要保留qwindows.dll文件即可。

**注意：**

若程序中使用了png以外格式的图片，在发布程序时就要将Qt安装目录下的plugins目录中的imageformats文件夹复制到发布程序文件夹中，其中只要保留自己用到的文件格式的dll文件即可。例如用到了gif文件，那么只需要保留qgif.dll。而如果程序中使用了其他的模块，比如数据库，那么就要将plugins目录中的sqldrivers文件夹复制过来，里面保留自己用到的数据库驱动。

这样程序就可以发布了，把整个文件夹或压缩包发给别人，别人就可以运行.exe程序了。

参考自Qt开源社区的Qt学习之路,http://www.qter.org/forum.php?mod=viewthread&amp;tid=629。
