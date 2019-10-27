---
title: ubuntu软件卸载方法
date: 2017-11-21
categories:
- 教程
tags:
- linux
toc: true
img: /medias/paperimg/bug.jpg
---

ubuntu软件卸载方法<!-- more -->
# 一、查看软件包
## 1.1 查看已安装的软件包
 ```
dpkg --list
```
## 1.2 查看不知道要删除软件的具体名称
```
dpkg --get-selections | grep <软件相关名称>
```

# 二、卸载

## 2.1 卸载程序和所有配置文件
```
sudo apt-get --purge remove <软件包名>
```
## 2.2 只卸载程序但保留配置文件
```
sudo apt-get remove <软件包名>
```