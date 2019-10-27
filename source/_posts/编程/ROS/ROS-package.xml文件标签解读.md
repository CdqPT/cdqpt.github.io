---
title: ROS-package.xml文件标签解读
date: 2019-1-7
categories:
- 笔记
tags:
- ros
toc: true
img: /medias/paperimg/ros.jpg

---
package.xml是一个XML文件名为package.xml中必须包括与任何兼容包的根文件夹。此文件定义有关包的属性，例如包名称，版本号，作者，维护者以及其他catkin包的依赖关系。<!-- more -->
|标签	|功能|
|:---:|:---:|
|<?xml> |	这是一个定义文档语法的语句，随后的内容表明在遵循xml版本 |
|<package>  |	从这个语句到最后</package>的部分是ROS功能包的配置部分 |
|<name>  	|功能包的名称。使用创建功能包时输入的功能包名称。正如其他|选项，用户可以随时更改。 |
| <version> |	功能包的版本,可以自由指定。 |
| <description> |	功能包的简要说明。通常用两到三句话描述 |
 |<maintainer>	 |提供功能包管理者的姓名和电子邮件地址 |
|<license>  |	 记录版权许可证。写BSD、MIT、Apache、GPLv3或LGPLv3即可|
 |<url> |	 记录描述功能包的说明，如网页、错误管理、存储库的地址等。根据功能包的类型，用户可以填写网站、错误跟踪（bugtracker）或存储库的地址|
| <author> 	|记录参与功能包开发的开发人员的姓名和电子邮件地址。如果涉及多位开发人员，只需在下一行添加<author>标签 |
| <buildtool_depend> |	描述构建系统的依赖关系。我们正在使用catkin 构建系统，因此输入catkin |
 |<build_depend> |	在编写功能包时写下您所依赖的功能包的名称 |
|<run_depend>  	|填写运行功能包时依赖的功能包的名称 |
| <test_depend> 	|填写测试功能包时依赖的功能包名称 |
|<export> |	 在使用ROS中未指定的标签名称时会用到<export>。最广泛使用的情况是元功能包的情况，这时用<export> <metapackage/> </export>格式表明是元功能包。 |
 |<metapackage> |	 在export标签中使用的官方标签声明，当前功能包为一个元功能包时声明它|