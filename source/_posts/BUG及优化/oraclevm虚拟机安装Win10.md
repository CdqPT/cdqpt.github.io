---
title: oraclevm虚拟机安装Win10
date: 2019-02-20
categories:
- 教程
tags:
- 虚拟机
toc: true
img: /medias/paperimg/bug.jpg
---

在deepin系统中使用oraclevm虚拟机安装Win10系统教程，当然在Windows中也是如此操作。<!-- more -->

# 一、新建虚拟电脑

填写名称、类型和版本。然后点击下一步。

![深度截图_VirtualBox_20190220092434.png](https://upload-images.jianshu.io/upload_images/16115686-33a28f256e29a0c7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 二、内存设置

内存调整建议不要超过实际内存的一半，不然会产生内存转换，使主机卡死。然后点击下一步。

![深度截图_VirtualBox_20190220092645.png](https://upload-images.jianshu.io/upload_images/16115686-70e1b9e6cb063490.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

之后一路默认就会在主菜单左侧出现虚拟电脑了。

![深度截图_VirtualBox Manager_20190220093429.png](https://upload-images.jianshu.io/upload_images/16115686-af7a3d65b52ef79d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


# 三、设置配置

## 3.1 主板配置

点击设置-系统-扩展特性：启用I/O APIC-ok。

![深度截图_VirtualBox_20190220093614.png](https://upload-images.jianshu.io/upload_images/16115686-365267661ad85c5a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 3.2 处理器配置
点击设置-系统-处理器-启用 PAE/NX-ok。

![深度截图_VirtualBox_20190220093902.png](https://upload-images.jianshu.io/upload_images/16115686-ff8eab8b5635a11b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 3.3 存储配置
点击存储-存储介质-没有光盘-属性-点击光盘图标选择要安装的系统镜像文件-ok。

![深度截图_VirtualBox_20190220094104.png](https://upload-images.jianshu.io/upload_images/16115686-7335528b14f446f7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 3.4 启动

在主菜单界面点击启动按钮。

![深度截图_VirtualBox Manager_20190220094250.png](https://upload-images.jianshu.io/upload_images/16115686-fdd376b8626b1e6d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

剩下的就等就可以了。
