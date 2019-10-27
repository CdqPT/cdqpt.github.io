---
title: Eclipse安装与测试
date: 2019-07-10 09:46:00
categories: 教程
tags: Java
toc: 
img: /medias/paperimg/java.jpg
summary: Eclipse安装与测试。
author: 陈德强
top: 
password: 
cover: 
---
Eclipse是一款Java编程的IDE。
# 一、Eclipese的安装
首先是下载：[Eclipse IDE for Eclipse Committers](http://www.eclipse.org/downloads/packages/)
然后正常安装就行。
# 二、Eclipse测试
File-》new-Java Project-Project name：test-Finish
找到左侧结构树，在test-src下右键-new-class-name：HelloWorld-finish
然后在Helloworld.java中填写代码：

```java
package newtest;

public class HelloWorld {
	public HelloWorld(String name)
	{
		System.out.println("小狗的名字是："+name);
		
	}
    public static void main(String []args) {
        HelloWorld helloworld=new HelloWorld("tydi");
    }
}
```
```
小狗的名字是：tydi
```

