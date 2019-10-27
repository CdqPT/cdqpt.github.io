---
title: 调整ubuntu/deepin系统的开机启动顺序
date: 2019-02-06
categories:
- 优化
tags:
- linux
toc: true
img: /medias/paperimg/bug.jpg
summary: 日常办公离不开Windows，而deepin和ubuntu又默认windows是第二启动项。
---
由于需求方面，经常使用ubuntu系统，后来发现deepin系统的UI设计十分惊艳，忍不住做了deepin双系统。但是日常办公离不开Windows，而deepin和ubuntu又默认windows是第二启动项，难道要每次开机都得等着选择启动项？NO!可以通过如下方式来解决：<!--more-->

# 一、调整ubuntu开机顺序

## 1.1 编辑启动文件
```
sudo gedit /boot/grub/grub.cfg
```
or
```
sudo -H gedit /boot/grub/grub.cfg &>/dev/null
```

## 1.2 调整开机顺序
剪切如下部分：
```
### BEGIN /etc/grub.d/30_os-prober ###
menuentry 'Windows 7 (loader) (on /dev/sda1)' --class windows --class os $menuentry_id_option 'osprober-chain-D4C4BF7AC4BF5E04' {
    insmod part_msdos
    insmod ntfs
    set root='hd0,msdos1'
    if [ x$feature_platform_search_hint = xy ]; then
      search --no-floppy --fs-uuid --set=root --hint-bios=hd0,msdos1 --hint-efi=hd0,msdos1 --hint-baremetal=ahci0,msdos1  D4C4BF7AC4BF5E04
    else
      search --no-floppy --fs-uuid --set=root D4C4BF7AC4BF5E04
    fi
    parttool ${root} hidden-
    chainloader +1
}
set timeout_style=menu
if [ "${timeout}" = 0 ]; then
  set timeout=10
fi
### END /etc/grub.d/30_os-prober ###
```
粘贴到如下代码前面：
```
### BEGIN /etc/grub.d/10_linux ###
```

# 二、调整deepin开机顺序


## 2.1 进入文件管理器
将/etc/grub.d文件夹下的30_os-prober文件改名为08_os-prober，此时可能需要获取管理员权限，deepin下直接右键就可以点击获取管理员权限了。
这里说明一下，grub.b文件下的数字就是启动顺序，10_是linux的启动顺序，所以08也可以改成06-09都可以。
##  2.2 更新文件
```
sudo update-grub
```
## 2.3 重启


参考自：

1. [https://www.cnblogs.com/softzrp/p/6715262.html](https://www.cnblogs.com/softzrp/p/6715262.html)

2. [https://blog.csdn.net/owen_suen/article/details/79050549](https://blog.csdn.net/owen_suen/article/details/79050549)