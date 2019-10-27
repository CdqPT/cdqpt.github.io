---
title: 博客-markdown语法笔记
date: 2019-02-15
categories:
- 字典
tags:
- 博客
- mrkdown
toc: true
img: /medias/paperimg/bug.jpg
---

markdown语法笔记分为初级语法（常用语法）和高级语法（炫技语法）<!-- more -->


# 一、初级语法
|作用   |语法        |
|:-------:|:-------:|
|标题 1|	`# Heading 1`|
|标题 2|	`## Heading 2`|
|标题 3|	`### Heading 3`|
|标题 4|	`#### Heading 4`|
|斜体   |`*italic*`|
|粗体   |	`**bold**`|
|删除文本|`~~删除文本~~`|
|行内标记|	`` `code` ``|
|图片   |	`![Image Title](http://xxx.png)`|
|链接   |	`[Link Text](http://xxx.com)`|

# 二、高级语法

## 2.1 引用
**单行引用**
```
> hello world!
```


效果如下：

> hello world!

---------------------------

**多行引用**
```
> hello world!
hello world!
hello world! 
```


效果如下：
> hello world!
hello world!
hello world! 

-------------------

**嵌套引用**

```
> aaaaaaaaa
>> bbbbbbbbb
>>> cccccccccc


```



效果如下：

> aaaaaaaaa
>> bbbbbbbbb
>>> cccccccccc

---------------------------


## 2.2 代码块

```
``` javascript
var num = 0;
for (var i = 0; i < 5; i++) {
    num+=i;
}
console.log(num);
```    


------------------------

效果如下：

``` javascript
var num = 0;
for (var i = 0; i < 5; i++) {
    num+=i;
}
console.log(num);
```
## 2.3 插入视频

```html
<video  height=498 width=510 preload=true  controls="controls" 
        src="http://blogcdq.oss-cn-beijing.aliyuncs.com/%E6%B5%8B%E8%AF%95%E8%A7%86%E9%A2%91.mp4" >
</video>
```

-----------------------------

其中，height是视频框架高度，width是视频框架宽度，src是视频地址，可从在线视频上的分享功能中提取。

效果如下:

<video  height=498 width=510 preload=true  controls="controls" 
 src="http://blogcdq.oss-cn-beijing.aliyuncs.com/%E6%B5%8B%E8%AF%95%E8%A7%86%E9%A2%91.mp4" >
	</video>
## 2.4表格

```
|    a    |       b       |      c     |
|:-------:|:------------- | ----------:|
|   居中  |     左对齐    |   右对齐   |
```

效果如下：

|    a    |       b       |      c     |
|:-------:|:------------- | ----------:|
|   居中  |     左对齐    |   右对齐   |

## 2.5 内嵌CSS样式

```
<p style="color: #AD5D0F;font-weight:bold;font-size: 30px; font-family: '宋体';" align="center">内联样式</p>
```

效果如下：

<p style="color: #AD5D0F;font-weight:bold;font-size: 30px; font-family: '宋体';" align="center">内联样式</p>




参考自：[https://www.jianshu.com/p/b03a8d7b1719](https://www.jianshu.com/p/b03a8d7b1719)