---
title: Python及包管理入门教程
author: 陈德强
date: 2019-07-21 12:32:00
categories:
- 教程
tags:
- python
toc: true
top: false
img: /medias/paperimg/python.jpg
summary: Python及opencv入门教程
---

# 一、安装anaconda
python最难搞得是环境配置。搞好环境配置才能事半功倍。anaconda是一款专门管理python环境的工具。
在[官网](https://www.anaconda.com/distribution/)下载对应版本的anaconda。注意平台和版本号。
一路默认安装。

# 二、安装pycharm
pycharm是一款热门的python编辑器。python编辑器有很多，但其他编辑器功能不完善。
在[官网](http://www.jetbrains.com/pycharm/download/#section=mac)下载社区版的pycharm。注意平台和版本号。
一路默认安装。

至此python安装完成。
下面是包管理教程。

# 三、更换anaconda镜像源
点击anaconda的environment，注意此后的所有操作都是在environment中进行。
creat一个的环境，例如命名为“anaconda_python27”。
然后更换anaconda中的镜像源，因为原来的镜像源好像不太好用。
点击右侧栏中Channel，Add，添加如下两个源（不知道哪个是好用的）：
```
https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/
https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
```

其他镜像源：
```
清华大学 https://pypi.tuna.tsinghua.edu.cn/simple/

阿里云 http://mirrors.aliyun.com/pypi/simple/

中国科技大学 https://pypi.mirrors.ustc.edu.cn/simple/

豆瓣(douban) http://pypi.douban.com/simple/

中国科学技术大学 http://pypi.mirrors.ustc.edu.cn/simple/
```

# 四、添加opencv和numpy
继续上续操作，将Installed改为All，
在搜索栏中搜索opencv，选中后Apply，这样就安装好了opencv，
nump安装同理。

# 五、在pycharm中创建新工程
打开pycharm，
file-new project
```
Location ：/Users/cdq/PycharmProjects/new
Project Interpreter-Exsting interpreter-...-Conda Envirornment-Interpreter-Python 3.7-Ok-Create
```
在Project下的空白处右键新建Python文件。

# 六、测试
```
import cv2

print(cv2.__version__)
```

运行后输出：
```
输出：3.4.1
```



