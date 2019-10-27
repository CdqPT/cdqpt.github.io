---
title: git笔记
author: 陈德强
date: 2019-10-08 19:36:00
categories:
- 笔记
tags:
- 其他
toc: true
top: false
img: /medias/paperimg/other.jpg
summary: git笔记。
---

# 一、简介
git将整个工程拷贝下来，就可以脱离远程服务器本地化改代码；
提交的时候是提交到本地，再提交到服务器，这保证了在无网的情况下依然可以提交代码；

# 二、安装
官网安装：
https://git-scm.com/downloads

# 三、用户信息
当安装完 Git 应该做的第一件事就是设置你的用户名称与邮件地址。 这样做很重要，因为每一个 Git 的提交都会使用这些信息，并且它会写入到你的每一次提交中，不可更改：
```
$ git config --global user.name "John Doe"
$ git config --global user.email johndoe@example.com
```
如果使用了 --global 选项，那么该命令只需要运行一次，因为之后无论你在该系统上做任何事情， Git 都会使用那些信息。 当你想针对特定项目使用不同的用户名称与邮件地址时，可以在那个项目目录下运行没有 --global 选项的命令来配置。

# 四、检查配置信息
```
git config --list
```

# 五、git add

如果新建了文件，就需要使用`git add`命令添加跟踪，不然git不会将其纳入更新范围。修改文件后也需要该命令才能加入暂存区。
 这是个多功能命令：可以用它开始跟踪新文件，或者把已跟踪的文件放到暂存区，还能用于合并时把有冲突的文件标记为已解决状态等。 将这个命令理解为“添加内容到下一次提交中”而不是“将一个文件添加到项目中”要更加合适。 

## 5.1 git commit
提交。
请记住，提交时记录的是放在暂存区域的快照。 任何还未暂存的仍然保持已修改状态，可以在下次提交时纳入版本管理。 每一次运行提交操作，都是对你项目作一次快照，以后可以回到这个状态，或者进行比较。

` git commit  -a `可以跳过`git add`步骤

# 六、git rm
移除。
如果只是简单地从工作目录中手工删除文件，运行 `git status` 时就会在 “Changes not staged for commit” 部分（也就是 未暂存清单）看到 该文件是未暂存状态，而使用 `git rm`后就不会再出现了。

# 七、分支

分支也就是版本。master一般是原始的分支。
你可以在不同分支间不断地来回切换和工作，并在时机成熟时将它们合并起来。
创建一个新分支就相当于往一个文件中写入 41 个字节（40 个字符和 1 个换行符）,与过去大多数版本控制系统形成了鲜明的对比，它们在创建分支时，将所有的项目文件都复制一遍。
当你切换分支的时候，Git 会重置你的工作目录，使其看起来像回到了你在那个分支上最后一次提交的样子。 Git 会自动添加、删除、修改文件以确保此时你的工作目录和这个分支最后一次提交时的样子一模一样。
创建分支
```
git branch testing
```
切换分支
```
git checkout testing
```
合并分支
```
git checkout master
git merge iss53
```

# 其他

可以为命令去别名
```
git config --global alias.co checkout
```



参考链接：
https://git-scm.com/book/zh/v2/%E8%B5%B7%E6%AD%A5-%E5%85%B3%E4%BA%8E%E7%89%88%E6%9C%AC%E6%8E%A7%E5%88%B6#ch01-introduction