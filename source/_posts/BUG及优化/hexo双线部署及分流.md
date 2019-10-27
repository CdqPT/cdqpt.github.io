---
title: hexo双线部署及分流
author: 陈德强
date: 2019-10-06 02:12:00
categories:
- 优化
tags:
- 博客
toc: true
top: false
img: /medias/paperimg/other.jpg
summary: hexo双线部署及分流。
---

github的服务器在国外，访问速度慢，而coding是国内的，访问比较快。
因此进行github和coding双部署，再进行分流操作，可以加快打开速度。

# 一、添加coding仓库
首先在根目录添加coding仓库：
```js
deploy:
- type: git
  repo:
    github: git@github.com:CdqPT/cdqpt.github.io.git
    coding: git@git.dev.tencent.com:chendeqiangsp/chendeqiangsp.coding.me.git
  brach: master

```
把本地生成 SSH 公匙添加到 Coding ,然后 hexo clean &&　hexo g && hexo d 部署上去就是了．

# 二、添加分流解析

我使用的是阿里云域名。
打开阿里云控制台 - 解析设置 - 添加记录
```
A
@
百度
124.156.1xx.2xx
```
```
A
@
谷歌
185.199.1xx.1xx
```
其中，记录值需要ipv4格式，可以先找到仓库的原域名，如：
github的在 
	仓库 - setting - Repository name - cdqpt.github.io
coding的在
	Pages - 服务设置 - 自定义域名 - chendeqiangsp.coding.me/chendeqiangsp.coding.me
然后将域名粘贴到ip工具里就可以获得ipv4了，也就是记录值。
https://www.iamwawa.cn/ip.html






