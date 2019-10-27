---
title: WPS for Linux字体配置（Ubuntu 16.04）
date: 2018-12-27
categories:
- bug
tags:
- linux
toc: true
img: /medias/paperimg/bug.jpg
---
解决缺少字体的BUG。<!-- more -->
## 错误提示：
 [![image](http://upload-images.jianshu.io/upload_images/16115686-794a1725a10045b4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](https://images2015.cnblogs.com/blog/417876/201707/417876-20170710163029837-1130169150.png "点击查看原图") 

## 解决方法：

从[http://bbs.wps.cn/thread-22355435-1-1.html](http://bbs.wps.cn/thread-22355435-1-1.html)下载字体库，离线版本：（链接: [https://pan.baidu.com/s/1i5dzB9r](https://pan.baidu.com/s/1i5dzB9r) 密码: pwe1）



在文件夹中打开终端使用命令解压：

```
sudo unzip wps_symbol_fonts.zip -d /usr/share/fonts/wps-office
```

解压完成后再次打开WPS就不会看到以上错误。

转载自：[http://www.cnblogs.com/EasonJim/p/7146587.html](http://www.cnblogs.com/EasonJim/p/7146587.html)
