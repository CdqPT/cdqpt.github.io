---
title: VM-安装MAC系统
date: 2019-1-25
categories:
- 教程
tags:
- 虚拟机
toc: true
img: /medias/paperimg/mac.jpg
---
搜了下论坛没有这个教程，继续搬运一波，这次教的是用VM15安装Mac OS10.14懒人版VMware安装Windows和Linux比较类似，相对于今天要安装的MAC OS来说过程也比较简单。官方原版VMware是不支持MAC OS安装的，但是外国大神制作的解锁工具让VMware安装MAC OS成为了可能，让我们去看看具体怎么安装的吧！<!-- more -->
安装准备：1：VMware Workstation Pro v15.0.0
2：解锁工具Unlocker v3.0.0
3：macOS Mojave 10.14懒人版
# 一、关闭VMware 
打开任务管理器，并找到后台进程，右键-结束所有VMware的进程（带VMware的就是）。
![image](http://upload-images.jianshu.io/upload_images/16115686-7a8c42c7b1993343.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 二、解锁
找到解锁工具，右键-以管理员身份运行win-install.cmd,脚本运行完毕会自动关闭。 
![image](http://upload-images.jianshu.io/upload_images/16115686-e3dcbc6f6ca9458f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 三、创建虚拟机
打开VMware，创建新的[虚拟机](https://www.52pojie.cn/thread-661779-1-1.html)，这次我选择“典型”。
![image](http://upload-images.jianshu.io/upload_images/16115686-08eaeeb0f7360058.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 四、安装macOS 
安装来源是macOS Mojave 10.14懒人版文件，注意浏览的时候文件类型要选择所有文件，不然找不到macOS Mojave 10.14 18A391 Lazy Installer.cdr
![image](http://upload-images.jianshu.io/upload_images/16115686-3f893d5f97318c01.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 五、注意
系统类型是MAC OS 10.14,如果没有以上的解锁操作，选项里是没有MAC OS可选的。 
![image](http://upload-images.jianshu.io/upload_images/16115686-d28c461c819cf403.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 六、安装位置
由于安装后系统文件会很大，我们要把默认的位置换成c盘以外的位置 。
![image](http://upload-images.jianshu.io/upload_images/16115686-7274983f827f2a45.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 七、修改配置文件
以下的几步默认即可，如果后续觉得配置不够，可以自行调整，配置完毕，不要急着打开虚拟机，找到刚才虚拟机系统文件路径下的macOS 10.14.vmx，用记事本打开，在 smc.present = "TRUE" 后添加（smc.version = "0"）(建议您复制，不包括括号) 后保存。 
![image](http://upload-images.jianshu.io/upload_images/16115686-41466d208bbffec1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image](http://upload-images.jianshu.io/upload_images/16115686-b5aca650b1cc6e55.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 八、等待安装
打开虚拟机，等待进度条加载完毕。 
![image](http://upload-images.jianshu.io/upload_images/16115686-c0eac49dd106c989.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 九、选择语言
语言选择简体中文，同意条款，继续安装。
![image](http://upload-images.jianshu.io/upload_images/16115686-24d675adb008b338.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image](http://upload-images.jianshu.io/upload_images/16115686-0ce7ff39a1d78bc4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 十、继续
到这一步点击上方“实用工具”里的磁盘工具 
![image](http://upload-images.jianshu.io/upload_images/16115686-2b120778726a805f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 十一、创建分区
单击左侧的vmware虚拟硬盘，然后找到上方的“编辑”-“抹掉”，名称就叫mac os吧 
![image](http://upload-images.jianshu.io/upload_images/16115686-740cf2500eb65906.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image](http://upload-images.jianshu.io/upload_images/16115686-7d08abe33e617c71.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 十二、选择分区
关闭磁盘工具，右侧会多出我们刚才分出来的一个磁盘，选择这个磁盘并继续。 
![image](http://upload-images.jianshu.io/upload_images/16115686-05819ce418801768.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image](http://upload-images.jianshu.io/upload_images/16115686-784f099295dca526.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 十三、用户配置
待安装完成，国家选择中国，键盘选择简体中文，不传输信息，apple id稍后设置，创建用户名和密码。
![image](http://upload-images.jianshu.io/upload_images/16115686-d4577850846d81ea.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image](http://upload-images.jianshu.io/upload_images/16115686-f244d0d87cb183e0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image](http://upload-images.jianshu.io/upload_images/16115686-701e99d15ec0778e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image](http://upload-images.jianshu.io/upload_images/16115686-33303bc60747ddcf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 十四、安装插件
全部设置完毕即可进入系统，首先推出安装磁盘，然后在vmware上方“虚拟机”选择安装vmware tools，安装完vmware tools重启系统即可实现全屏和文件共享功能（如果提示系统扩展被阻挡，请在偏好设置—安全与隐私中选择允许再次安装）。 
![image](http://upload-images.jianshu.io/upload_images/16115686-01cb5002870a10d6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

教程到此结束啦，附上链接: [https://pan.baidu.com/s/1mvDbcvaLtUz3eXQ6tYVRrg](https://pan.baidu.com/s/1mvDbcvaLtUz3eXQ6tYVRrg) 提取码: 8tc8看到这里的小伙伴记得给个评分哦，评分不减CB！！！

转载自：https://www.52pojie.cn/thread-804000-1-1.html

