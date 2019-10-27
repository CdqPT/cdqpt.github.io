---
title: MAC安装boost库
author: 陈德强
date: 2019-09-28 10:25:00
categories:
- 教程
tags:
toc: true
top: false
img: /medias/paperimg/c.jpg
summary: boost安装教程。
---

# 一、下载boost库
https://www.boost.org/

# 二、解压
# 三、编译
在解压后的文件夹内打开终端，执行：
```
./bootstrap.sh
sudo ./b2 --buildtype=complete install
```
# 四、添加库文件位置
安装好后，Xcode的项目中还是找不到Boost，需要手动将Boost的路径导入进去。
点击左侧工程名称，在右侧的Build Settings标签里点击ALL找到其中的Search Paths下的Header Search Paths一栏，双击增加一个目录，填入目录位置，/usr/local/include/，
然后找到Library Search Paths一栏，填入/usr/local/lib，
这样就能正常调用Boost库了。

# 五、测试
```c
#include <iostream>
#include <boost/version.hpp>

int main(int argc, const char * argv[]) {


    std::cout<<"Boost版本："<<BOOST_VERSION<<std::endl;
    

    return 0;
}

```
```
Boost版本：107100
Program ended with exit code: 0
```

参考链接：
https://www.jianshu.com/p/7ab8ac4cb0ad
https://blog.csdn.net/nick_666/article/details/77584900
https://www.cnblogs.com/linjk/p/6052886.html