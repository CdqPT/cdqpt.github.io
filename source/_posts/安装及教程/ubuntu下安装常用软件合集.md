---
title: ubuntu下安装常用软件合集
date: 2018-12-26
categories:
- 教程
tags:
- linux
toc: true
img: /medias/paperimg/ubuntu.jpg
summary: 包括QQ，微信，搜狗输入法，百度网盘等。
---

# 一、安装谷歌浏览器

## 1.1 下载并安装google浏览器

Ubuntu版本Google浏览器下载链接：[http://www.ubuntuchrome.com/](http://www.ubuntuchrome.com/)

## 1.2 安装谷歌访问助手

谷歌访问助手链接：[http://www.ggfwzs.com/](http://www.ggfwzs.com/)

# 二、安装搜狗输入法

## 2.1 下载搜狗输入法Linux版软件包

下载地址为：[http://pinyin.sogou.com/linux/](http://pinyin.sogou.com/linux/) ,如下图，要选择与自己系统位数一致的安装包，我的系统是64位，所以下载64位的安装包。

![image](http://upload-images.jianshu.io/upload_images/16115686-71379847abec758a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



## 2.2 更新软件库
先更新软件库

```
sudo apt-get update
sudo apt-get upgrade
```
切换目录
```
cd ~/下载/ 
或
cd ~/Downloads/
```

## 2.3 安装软件包

```
sudo dpkg -i sogoupinyin_2.1.0.0082_amd64.deb  
```

将绿色部分替换为安装包的名称。

![image](http://upload-images.jianshu.io/upload_images/16115686-8ee861a7b06e1c11?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 2.4 安装依赖

```
sudo apt-get install -f  
```

![image](http://upload-images.jianshu.io/upload_images/16115686-d1e838bc91b9b0f0?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

把系统键盘输入方式从ibus切换为fcitx，一般情况下默认就是fcitx，因此大多数情况下不用切换。

![image](http://upload-images.jianshu.io/upload_images/16115686-511e4f4b3e15dbec?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

重启

打开`fcitx config`，取消`仅当前语言`，添加搜狗输入法。

将搜狗输入法拖到置顶
没有企鹅也是正常的

再次重启

# 三、安装日常办公软件

包括QQ,TIM,微信，百度网盘等。

## 3.1 安装deepin-wine环境
上[https://github.com/wszqkzqk/deepin-wine-ubuntu](https://github.com/wszqkzqk/deepin-wine-ubuntu)页面下载zip包（或用git方式克隆），在“下载”目录下原地解压即可，

在文件夹中**右键**打开终端，输入

```
sudo sh ./install.sh
```
## 3.2 安装deepin.com应用容器
在[http://mirrors.aliyun.com/deepin/pool/non-free/d/](http://mirrors.aliyun.com/deepin/pool/non-free/d/)中下载想要的容器，点击deb安装即可。以下为推荐容器:

*   QQ：[http://mirrors.aliyun.com/deepin/pool/non-free/d/deepin.com.qq.im/](http://mirrors.aliyun.com/deepin/pool/non-free/d/deepin.com.qq.im/)
*   TIM：[http://mirrors.aliyun.com/deepin/pool/non-free/d/deepin.com.qq.office/](http://mirrors.aliyun.com/deepin/pool/non-free/d/deepin.com.qq.office/)
*   QQ轻聊版：[http://mirrors.aliyun.com/deepin/pool/non-free/d/deepin.com.qq.im.light/](http://mirrors.aliyun.com/deepin/pool/non-free/d/deepin.com.qq.im.light/)
*   微信：[http://mirrors.aliyun.com/deepin/pool/non-free/d/deepin.com.wechat/](http://mirrors.aliyun.com/deepin/pool/non-free/d/deepin.com.wechat/)
*   Foxmail：[http://mirrors.aliyun.com/deepin/pool/non-free/d/deepin.com.foxmail/](http://mirrors.aliyun.com/deepin/pool/non-free/d/deepin.com.foxmail/)
*   百度网盘：[http://mirrors.aliyun.com/deepin/pool/non-free/d/deepin.com.baidu.pan/](http://mirrors.aliyun.com/deepin/pool/non-free/d/deepin.com.baidu.pan/)
*   360压缩：[http://mirrors.aliyun.com/deepin/pool/non-free/d/deepin.cn.360.yasuo/](http://mirrors.aliyun.com/deepin/pool/non-free/d/deepin.cn.360.yasuo/)
## 3.3 安装.deb安装包

```
cd ~/下载

sudo dpkg -i sogoupinyin_2.1.0.0082_amd64.deb
```

注：-i后面替换成需要安装的.deb文件名。

# 四、安装sublime

##第一种方式
下载Linux安装包:http://www.sublimetext.com/3

得到:sublime_text_3_build_3200_x64.tar.bz2

解压:
```bash
tar -xvvf sublime_text_3_build_3200_x64.tar.bz2 sublime_text_3/
```
```bash
sudo mv sublime_text_3/ /opt
```
创建链接:
```bash
sudo ln -s /opt/sublime_text_3/sublime_text /usr/bin/sublime_text
```
创建图标:
```bash
ln -s /opt/sublime_text_3/sublime_text /usr/bin/sublime_text ~/Desktop
```
即可在桌面看到相应的快捷方式。这里前一个路径是程序的源路径，第二个是需要创建快捷方式的地方。

## 第二种方式
### 安装密匙

```
wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg | sudo apt-key add -
```

### 确定用网址源下载
```

sudo apt-get install apt-transport-https
```

### 选择稳定版

```
echo "deb https://download.sublimetext.com/ apt/stable/" | sudo tee /etc/apt/sources.list.d/sublime-text.list
```

### 安装sublime

```
sudo apt-get install sublime-text
```

参考链接：
https://www.cnblogs.com/hxzkh/p/7827777.html