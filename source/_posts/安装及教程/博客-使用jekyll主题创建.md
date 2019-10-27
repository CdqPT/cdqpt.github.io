---
title: 博客-使用jekyll主题创建
date: 2019-02-04
categories:
- 教程
tags:
- 博客
toc: true
img: /medias/paperimg/other.jpg

---

为什么要写博客呢？

因为程序员最擅长的是复制粘贴啊哈哈哈！<!-- more -->

为什么要建立个人博客呢？

因为.....可以自定义？哈哈

不管初衷是什么吧，这个博客搭建起来不是很容易，所以记录一下。<!-- more -->

# 一、注册github账号
参考链接[https://jingyan.baidu.com/article/455a9950abe0ada167277864.html](https://jingyan.baidu.com/article/455a9950abe0ada167277864.html)
# 二、获取模板
模板链接：[http://jekyllthemes.org/](http://jekyllthemes.org/)
## 2.1 选择一个模板后点进去
![image.png](https://upload-images.jianshu.io/upload_images/16115686-ab7737712246c3a6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
## 2.2 点击fork
![image.png](https://upload-images.jianshu.io/upload_images/16115686-2f30931529315c24.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
## 2.3 点击Settings
![image.png](https://upload-images.jianshu.io/upload_images/16115686-2c41fec0f0f06955.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
## 2.4 重命名
重命名为何自己账户一样的名字，账户名就是左上角圈中的地方,注意要小写，而且是.github.io/结尾。如果想修改账户名，可参考：[https://jingyan.baidu.com/article/49711c61b01fcafa451b7c4f.html](https://jingyan.baidu.com/article/49711c61b01fcafa451b7c4f.html)
## 2.5 访问
Rename后就可以通过Setting下方的GitHub Pages链接在任意网页上直接访问你的博客了！
![image.png](https://upload-images.jianshu.io/upload_images/16115686-42eed8cef5d834a6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
# 三、绑定域名
## 3.1 去阿里云购买域名
参考链接：[https://jingyan.baidu.com/article/09ea3ede7989d4c0aede39ef.html](https://jingyan.baidu.com/article/09ea3ede7989d4c0aede39ef.html)
## 3.2 记录CNAME
在github仓库根目录创建名字为CNAME的文件（无后缀），填写自己的域名，并放在public文加夹中：
![image.png](https://upload-images.jianshu.io/upload_images/16115686-cb5df4b29cfcf8ef.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
## 3.3 绑定域名
点解析
![image.png](https://upload-images.jianshu.io/upload_images/16115686-afaf2501781bde9a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
添加CAME纪录

![image.png](https://upload-images.jianshu.io/upload_images/16115686-5447c6421b12e77f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
修改内容：
```
记录类型：A记录
主机记录：@
解析线路：默认
记录值：填写自己的github的ip
```
ip可以用windows的cmd命令输入命令获得。
```
ping cdqpt.github.io
```
如果不知道自己的仓库地址，可以进入仓库后看地址栏查找
![image.png](https://upload-images.jianshu.io/upload_images/16115686-48f714e850b97517.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
这样过两分钟就可以通过购买的域名访问自己的博客了！
如果几分钟后仍是404页面，那么可以尝试将http换成https来访问：
```
例如：http://purethought.cn换成https://purethought.cn
```
# 四、如何撰写博客
git写博客并不方便，网上都极力推荐Hexo或 jekyll ，我搞了半天，终于从入门到放弃了，太难用了。
于是选择了用简书作为书写平台（一波硬广告），可以实时预览页面情况而且图片可以直接复制上去，然后把简书上的Markdown贴到github的博文里即可。
![image.png](https://upload-images.jianshu.io/upload_images/16115686-3962b66c6509fae1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
注意事项：
1.github的博文储存在_posts文件夹中，如果丢失了这个文件夹，可以新建文件`_posts/`就会出现了。
2.博文命名格式必须是`2019-02-02-名字.md`这样的格式。
# 五、头信息
头信息可以帮助我们给博客自动分类。
常用的有以下几个键值对：
```
---
title: My blog title
date: 2017-08-11
categories:
- life
- more
tags:
- blog
- post
---
```
`date`指定了文章写作日期；`categories`指定了文章放置的目录；`tags`指定文章标签。这些信息书写后，Jekyll会自动将你的文章按时间顺序收录和生成标签云。是不是很赞~关于Jekyll以及YAML的相关知识，可以查看[官方中文文档](http://jekyllcn.com/docs/home/)。

参考链接：
1.[https://www.jianshu.com/p/ba81a536d61a](https://www.jianshu.com/p/ba81a536d61a)
2.[https://blog.csdn.net/xudailong_blog/article/details/78762262](https://blog.csdn.net/xudailong_blog/article/details/78762262)
