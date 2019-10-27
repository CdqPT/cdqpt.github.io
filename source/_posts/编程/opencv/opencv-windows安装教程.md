---
title: opencv-windows安装教程
author: 陈德强
date: 2019-10-05 21:41:00
categories:
- 教程
tags:
- opencv
toc: true
top: false
img: /medias/paperimg/opencv.jpg
summary: opencv-windows安装教程。
---



# 一、下载opencv
下载链接：
https://opencv.org/releases/




# 二、运行exe

运行exe（其实是解压），将压缩包解压到相应目录，如：
D:\Program Files (x86)\opencv

# 三、添加环境变量

根据安装目录新建环境变量：

此电脑 - 属性 - 高级系统设置 - 高级 - 环境变量 - 系统变量(s) - path - 双击 - 新建 - `D:\Program Files (x86)\opencv\build\x64\vc15\bin`

# 四、新建vs2019工程

如 ：Test

# 五、添加VC++目录

解决方案管理器 - Test - 右键 - 属性 - 配置属性 - VC++目录 - 平台（x64）
- 包含目录 - `D:\Program Files (x86)\opencv\build\include` 和 `D:\Program Files (x86)\opencv\build\include\opencv2` 
- 库目录 - `D:\Program Files (x86)\opencv\build\x64\vc15\lib`

解决方案管理器 - Test - 右键 - 属性 - 配置属性 - 链接器 - 输入 - 平台（x64） - 附加依赖项 - `opencv_world411d.lib`

确定。

# 六、测试

vs2019 Debug x64 环境
```c
#include<iostream>
#include <opencv2/core/core.hpp>
#include <opencv2/highgui/highgui.hpp>

using namespace cv;

int main()
{
   // 读入一张图片（游戏原画）
   Mat img = imread("C:/Users/Administrator/Desktop/pic.jpg");
   // 创建一个名为 "游戏原画"窗口
   namedWindow("游戏原画");
   // 在窗口中显示游戏原画
   imshow("游戏原画", img);
   // 等待6000 ms后窗口自动关闭
   waitKey(0);
}
```

弹出图片就对了！

参考链接：
https://blog.csdn.net/duwangthefirst/article/details/79452314




