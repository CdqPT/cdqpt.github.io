---
title: Ubuntu常用命令
date: 2019-10-09 09:53:00
categories: 字典
tags: linux
toc: true
img: /medias/paperimg/linux.jpg
summary: 镜像源 mirrors.aliyun.com
author: 陈德强
top: true
password: 
cover: 

---

# 一、常用命令

|命令|作用|
|:---:|:---:|
ifconfig |显示当前网络ip
执行bash文件|./ xxx.bash
gedit file.txt|编辑文件
ps -e\|grep 关键字|查看进程号
cat /etc/issue |查看系统版本
sh XXXX.sh| 安装.sh软件包 
wget deb 安装包网址| 下载安装包 
sudo dpkg -i xxxx.deb| 安装.deb软件包 
sudo apt-get -f install| 安装依赖 
sudo apt-get install XXXXX| 安装软件库的软件 
sudo apt-get remove XXXXX| 卸载软件库的软件 
sudo apt-get remove -purge XXXXX|卸载并清除配置 
sudo apt-get update| 更新软件列表 
sudo chmod 777 ××× |每个人都有读和写以及执行的权限
sudo chmod 600 ××× |只有所有者有读和写的权限
which 程序名|返回可执行文件位置
locate 文件名|查找文件，部分匹配
ctrl+h|显示隐藏文件
xrandr -s 1440x900 -r 60|设置分辨率  


</br>
 
`获取root权限`

```java
//设置root密码
sudo passwd root
//输入两遍密码
//获取root权限
su root
```

# 二、终端常用命令

|命令|作用|
|:---:|:---:|
Ctrl+a |移动到当前行的开头
Ctrl+l|刷新屏幕
Ctrl-c|终止当前正在运行的程序。
ctrl+shift+c|复制
ctrl+shift+v|粘贴
Ctrl+a |移动到当前行的开头
Ctrl+c |删除整行
ctrl+alt+t|打开终端
Shift+Ctrl+t|新建标签页
Shift+Ctrl+w|关闭标签页
Ctrl+PageUp|前一标签页
Ctrl+PageDown|后一标签页
F11|全屏


# 三、vim常用命令
 vi/vim 共分为三种模式，分别是命令模式（Command mode），输入模式（Insert mode）和底线命令模式（Last line mode）。 

一般流程：
>`vi runoob.txt`，打开文档
进入一般模式.
按下 i 进入输入模式(也称为编辑模式)，开始编辑文字，键盘上除了 Esc 这个按键之外，其他的按键都可以视作为一般的输入。
按下 ESC 按钮回到一般模式
在一般模式中按下 `:wq` 储存后离开 vi
>


# 四、系统相关

|命令|作用|
|:---:|:---:|
|uname -a |显示当前系统相关信息|
|sudo |临时获取超级用户权限|
|su root |切换 root 用户
|sudo shutdown |关机
|sudo reboot| 重启
|ifconfig |网络配置，IP 地址查看
|sudo nautilus |进入有 root 权限的文件管理器
|ps -A |查看当前有哪些进程
|kill 5 |位进程号 结束进程
|sudo fdisk -l |查看磁盘信息
|sudo mount /dev/sdb1 /mnt |挂载磁盘到某一路径
|sudo mount -r /dev/sdb1 /mnt/ |以只读方式挂载
|sudo umount /dev/sdb1 |卸载磁盘
|sudo blkid |查看磁盘分区的 UUID
|sudo vi /etc/fstab |开机自动挂载磁盘
|efibootmgr |查看系统启动顺序
|man command-name |查找命令详细手册
|command-name --help| 查找某一命令的帮助

</br>

`设置静态 IP 地址`

```c
sudo vi /etc/network/interfaces
//添加以下内容
auto enp129s0f1
iface enp129s0f1 inet static
address 192.168.1.254 # IP 地址
gateway 192.168.1.1 #
netmask 255.255.255.0 # 子网掩码
dns-nameservers 8.8.8.8 8.8.4.4 # DNS 解析
```

# 五、用户及权限管理

|命令|作用|
|:---:|:---:|
|sudo adduser username |新添加用户
|sudo passwd root |设置 root 用户密码
|chown user-name filename |改变文件的所属用户
|chmod u+rwx g+r o+r filename |用户添加读写运行权限，组成员添加读权限，其他用户添加读权限
|chmod a+w filename |所有用户添加写权限
|chmod 777 filename |所有用户添加读写运行权限
|chmod 644 filename |只有属主有读写权限；而属组用户和其他用户只有读权限。

</br>

`赋予新用户 root 权限`
```c
sudo vim /etc/sudoers
```
```c
# User privilege specification
root ALL=(ALL:ALL) ALL
username ALL=(ALL:ALL) ALL   新添加此行即可
```

# 六、软件安装

|命令|作用|
|:---:|:---:|
|sudo apt-get update |更新软件列表，在文件 /etc/apt/sources.list 中列出
|sudo apt-get upgrade |更新软件
|sudo apt-get install software-name| 安装在软件库中的软件
|sudo apt-get remove |卸载软件
|sudo apt-get purge |卸载软件并删除配置文件
|sudo apt-get clean |清除软件包缓存
|sudo apt-get autoclean |清除缓存
|sudo apt-get autoremove |清除不必要的依赖
|sudo apt-get install -f |修复安装依赖问题
|sudo dpkg -i *.deb |安装 deb 软件
|dpkg -l |查看所有安装的软件
|dpkg -l \| grep software-name |配合 grep 命令可查看具体的某一个软件是否安装
|sudo echo "google-chrome-stable hold" \| sudo dpkg --set-selections |不更新某个软件
|sudo echo "google-chrome-stable install" \| sudo dpkg --set-selections| 恢复更新某个软件

# 七、 目录文件操作

|命令|作用|
|:---:|:---:|
|cd| 切换目录，～为home目录，/为根目录，./为当前目录
|cd .. |切换到上级目录
|cd - |切换到上一次所在的目录
|pwd |查看当前所在目录
|ls| 查看当前目录下的文件夹和文件名，-a显示隐藏文件，-l显示文件详细信息
|mkdir directory-name| 新建文件夹
|rmdir directory-name |删除文件夹(必须为空)
|rm -rf directory-name| 强制并递归删除文件夹
|cp src-file dst-file |复制文件
|mv src-file dst-file |移动文件
|find path -name string |查找路经所在范围内满足字符串匹配的文件和目录
|cat filename |显示文件内容
|head -n 2 filename |显示文件前两行的内容
|tail -n 2 filename |显示文件末尾两行的内容
|ln -s src-file dst-file |建立软链接

# 八、终端快捷键

|命令|作用|
|:---:|:---:|
|ctrl + l| 清屏
|ctrl + c |终止命令
|ctrl + d |退出 shell
|ctrl + z |将当前进程置于后台，fg 还原
|ctrl + r |从命令历史中找
|ctrl + u |清除光标到行首的字符（还有剪切功能）
|ctrl + w |清除光标之前一个单词 （还有剪切功能）
|ctrl + k |清除光标到行尾的字符（还有剪切功能）
|ctrl + y |粘贴 Ctrl+u 或 Ctrl+k 剪切的内容
|ctrl + t |交换光标前两个字符
|Alt + d |由光标位置开始，往行尾删删除单词
|Alt + . |使用上一条命令的最后一个参数
|Alt – b \|\| ctrl + 左方向键 |往回(左)移动一个单词
|Alt – f \|\| ctrl + 右方向键 - |往后(右)移动一个单词
|!! |执行上一条命令。



</br>
</br>

参考链接：
https://blog.csdn.net/seniusen/article/details/81055642
https://www.jianshu.com/p/2c3a086196c3
https://www.runoob.com/linux/linux-vim.html