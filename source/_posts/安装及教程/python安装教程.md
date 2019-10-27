---
title: python安装教程
date: 2019-04-14 16:14:00
categories: 教程
tags: python
toc: true
img: /medias/paperimg/python.jpg
summary: 使用anacond集成包安装
author: 陈德强
top: 
password: 
cover: 
---
python不难学，难的是配置环境，所以一定要有包管理器才能事半功倍，推荐使用pycharm或anaconda，首推pycharm。

# 一、使用pycharm
windows安装pycharm就不介绍了，因为很简单。介绍一下debian下pycharm的安装和配置。
## 1.1 下载
[下载链接](http://www.jetbrains.com/pycharm/)

下载完解压，进入bin文件夹，在该目录下运行命令行，命令中的pycharmXXXXX.sh改成和你文件一样的名字。
```
sh pycharmXXXXX.sh
```

安装后就要配置环境了，我这里使用的是python3.5环境。

首先更改软件源，目的是下载的更快：
file-setting-project-interpret，右下方选择加号，点击下方的manage repository，把里面默认的删掉，添加新的网址
```
清华: https://pypi.tuna.tsinghua.edu.cn/simple

豆瓣: http://pypi.douban.com/simple/

阿里: http://mirrors.aliyun.com/pypi/simple/
源网：https://pypi.python.org/simple
```

file-setting-project-interpret，然后在右上方的project interpret里选择Python3.5 /usr/bin/python3.5
然后再搜索并安装opencv-contrib-python。

配置环境真是害死人啊～

# 二、使用anaconda包管理软件
## Windows安装anaconda
Anaconda [官方下载链接](https://www.continuum.io/downloads)
下载EXE程序双击安装即可。

## ubuntu安装anaconda
### 2.1 下载
Anaconda [官方下载链接](https://www.continuum.io/downloads)

### 2.2 改名
讲下载后的文件改成Anaconda.sh
### 2.3 安装
```
cd ~/下载
```
```
sudo bash Anaconda.sh 
```
回车直到出现‘yes’or'no'
输入  yes
然后出现>>>
按回车
开始安装
### 2.4 添加环境变量
安装完后会提示是否添加环境变量输入yes
[no]>>>yes
```
sudo gedit ~/.bashrc
export PATH="/home/xupp/anaconda3/bin:$PATH"
```

### 2.5 测试
关掉当前终端，重开一个终端。
输入Python,会提示是Python3.7版本，而不是自带的2.x版本。

### 2.6 启用Spyder编辑器
```
cd ~/anaconda3/bin
```
```
./spyder
```