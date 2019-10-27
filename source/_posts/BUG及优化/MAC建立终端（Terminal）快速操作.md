---
title: MAC建立终端（Terminal）快速操作
date: 2019-05-05 18:19:00
categories: 优化
tags: mac
toc: true
img: /medias/paperimg/bug.jpg
summary: MAC建立终端（Terminal）快速操作
author: 陈德强
top: 
password: 
cover: 
---

找到`Automator`(中文名字叫“自动操作”)，
双击新建“快速操作”（就版本叫“服务”），
然后从左边选框中选择“运行 AppleScript”，把它拖进右边空白区域，
在窗口上部的““服务”收到选定的”右边下拉菜单选择“没有输入”，
然后可以把“图像”更改为“编写”，这样具有标识度，
然后修改脚本，
```
on run {input, parameters}
	
	(* Your script goes here *)
	tell application "Terminal"
		reopen
	end tell
	
	return input
end run
```
按住“command + s”取个名称“Terminal”保存脚本。
系统偏好设置
键盘
快捷键
服务
通用
勾选Terminal
同时添加快捷键即可。

下面将其添加到快速操作中，
系统偏好设置
扩展
触控栏
勾选Terminal

系统偏好设置
键盘
键盘
触控栏显示“快速操作”
ok！这时在触控栏就看见新建的Terminal快速操作了

