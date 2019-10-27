
---
title: ROS安装教程
date: 2017-11-17
categories:
- 教程
tags:
- ros
toc: true
img: /medias/paperimg/ros.jpg

---
对于ROS的安装，在它的官方网站: [http://wiki.ros.org/ROS/Installation](http://wiki.ros.org/ROS/Installation "ROS官方网站安装教程") 中也有详细说明。但是对于像博主这样先天英语发育不全的人来说，直接看官网还是有点困难的。

所以博主痛定思痛，经过一番呕心沥血与含辛茹苦的调研后(其实就是看了几篇相关博客)，终于在博主的电脑上成功安装了ROS，下面就是博主安装的全过程及所遇到的坑坑包包...<!--more-->

# 一、版本选择

ROS 虽说也叫操作系统，但它是寄生在 LINUX 操作系统之下的，所以要求大兄弟你的电脑里至少要先有一个 LINUX 操作系统。

而对 ROS 兼容性最好的当属 Ubuntu 操作系统了，所以大兄弟，嘿嘿嘿，你自己看着办！

首先有一点需要说明，ROS是用来干“大事业”的，所以不推荐也不认同更不接受大家使用虚拟机。之前博主抱着玩一玩ROS的态度，在虚拟机里装了Ubuntu， 然后装ROS，结果，结果，结果被ROS给玩了...

## 1.1 Ubuntu 和 ROS 版本对应

即便是大兄弟用了Ubuntu，也是不能随便找一个版本的ROS装滴...

为啥呢，因为 Ubuntu 和 ROS 都是存在不同的版本滴，而且ROS各个版本之间还很接地气的(谁说的，打死他)互不兼容，所以每一个 ROS 版本都对应着一个或两个对应的 Ubuntu 版本。

具体咋对应的？请看：

| ROS发布日期 | ROS版本 | 对应Ubutnu版本 |
|  :------: |  :------: | :------: |
| 2016.3 | ROS Kinetic Kame | Ubuntu 16.04 (Xenial) / Ubuntu 15.10 (Wily) |
| 2015.3 | ROS Jade Turtle | Ubuntu 15.04 (Wily) / Ubuntu LTS 14.04 (Trusty) |
| 2014.7 | ROS Indigo Igloo| Ubuntu 14.04 (Trusty) |
| 2013.9 | ROS Hydro Medusa| Ubuntu 12.04 LTS (Precise) |
| 2012.12 | ROS Groovy Galapagos| Ubuntu 12.04 (Precise)  |
|  ... | ... |  ... |

所以大兄弟，看到了吧，如果系统版本和ROS版本不对应，那是万万装不上滴。。。博主此处已嫩牛满面。。。

## 1.2 博主的配置

据博主的不完全统计(压根就没统计)的数据显示，现在学ROS的兄弟们普遍安装的是 Indigo 和 Hydro 版本, 但是现在已经时2016年啦，新的一年就要有新气象，所以，博主就能别人所不能(呵呵)，安装了Kinetic。

具体配置如下：

#### 华硕笔记本 + Windows 10 + Ubuntu 16.04 双系统

*   Ubuntu 硬盘大小: 100G
*   内存: 8G
*   显卡: 也不是用来打dota， 所以随便啦啦啦~\(≧▽≦)/~啦啦啦

#### Ros版本：ROS Kinetic Kame

博主分别用过 Indigo 和 Kinetic，其实在使用过程中差距并不大，除了极少数第三方库，只支持 Indigo版本，毕竟 Kinetic 刚刚发布，存在一些第三方库还没有及时跟进啦。。。

# 二、开始安装

既然选定好版本，我们就开始安装啦！

前提还是大兄弟已经自己安装好了 Ubuntu 16.04 哦！如果是 Ubuntu 14.04，只需要把下面所有出现 *-kinetic-* 的地方换成 *-indigo-* 就好了。

## 2.1 添加源

打开一个控制台(Ctrl + Alt + T), 添加输入如下指令：

```
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
```

设置秘钥:

```
sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net:80 --recv-key 0xB01FA116
```

## 2.2 安装 ROS

首先确保系统软件处于最新版

```
sudo apt-get update
```
然后我们就可以安装 ROS 啦，但是问题又出现了，ROS kinetic 也有很多版本，比如工业版，基础版，高级版，豪华版，至尊豪华...

既然我们想要学习ROS，那就安装至尊豪华全功能版吧，指令如下：

```
sudo apt-get install ros-kinetic-desktop-full
```

好，打完指令，就可以整瓶啤酒，撸个烤串，看看电视消遣消遣，坐等ROS安装完成。

如果大兄弟家的网够快的话，没准分分钟就完事儿了。。。

...3...

...2...

...1...

倒数三个数，好，现在就当大兄弟安装完了，而且一切顺利，没有小虫子(BUG)粗现...

安装完成后，可以用下面的命令来查看可使用的包：

```
apt-cache search ros-kinetic
```

到现在，虽然是安装完了，但是还不能用ROS哦，大兄弟别着急，心急吃不到豆腐...哦，是吃不到热豆腐...

## 2.3 初始化ROS

首先呢，需要先初始化 rosdep，嗯？这是啥？这不就是那个啥嘛，对吧，哈哈哈。。。⊙﹏⊙b汗

具体如下：

```
sudo rosdep init
rosdep update
```

------------------------------

此处可能有坑！！！

```
可能会返回如下错误：
ERROR：cannot download default sources list from:
https://raw.githubusercontent.com/ros/rosdistro/master/rosdep/sources.list.d/20-default.list
Website may be down
```

![这里写图片描述](http://upload-images.jianshu.io/upload_images/16115686-2a3cfc8a286df7f5?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

解决办法：

```
sudo c_rehash /etc/ssl/certs
sudo -E rosdep init
rosdep update
```

![这里写图片描述](http://upload-images.jianshu.io/upload_images/16115686-208fa60dbbdadfaa?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

--------------------------------------

接上（$ rosdep update），然后初始化环境变量:

```
echo "source /opt/ros/kinetic/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

上面两句非常非常非常重要，很多小伙伴在日常的开发过程中，有的找不到 Package, 找不到node, 很多情况下都是没有添加source, 这里展开说就有点跑偏了，如果小伙伴们遇到问题，可以在留言中提出来...

最后呢，对，这是最后的最后了，安装一个非常常用的插件：

```
sudo apt-get install python-rosinstall
```

好，到这里，所有安装就都完事啦。。哈哈哈。。为了保险，重启一下，测试测试我们的ROS吧....

# 三、测试ROS

安装完了好歹要测试一下吧，不然怎么对的起那瓶啤酒啊...大兄弟，你还清醒吗...

首先，启动ROS环境

```
roscore
```
看看显示 started core service [/rosout]  了没有？如果没问题，恭喜大兄弟，你成功了。

什么？出问题了？那好吧，估计是啤酒喝多了，再从头来一遍吧，这次就别喝了。。。

参考自：[http://www.cnblogs.com/liu-fa/p/5779206.html#3536022](http://www.cnblogs.com/liu-fa/p/5779206.html#3536022)
