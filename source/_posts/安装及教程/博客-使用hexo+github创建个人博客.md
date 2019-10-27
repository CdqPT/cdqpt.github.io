---
title: 博客-使用hexo+github创建个人博客
date: 2019-02-06
categories:
- 教程
tags:
- 博客
toc: true
img: /medias/paperimg/other.jpg
---

# 一、在Win下安装
## 安装Git Bash
git bash是github的命令行，类似于cmd，用于输入指令。下载地址：

[https://git-for-windows.github.io/](https://git-for-windows.github.io/)

一路默认安装就可以。

## 安装NodeJs
不知道干啥的，反正里面有个npm工具。下载地址：

https://nodejs.org/en/

安装成功后，可以用命令查看是否成功：
```
node -v
```
如果出现版本号，就代表成功了。

## 安装hexo
hexo是一种依托于github的博客生成软件，简言之就是构建博客网站的一种软件。

在任意地方创建新文件夹用于存放博客（我的叫GitRep），然后再该文件夹内右键打开**git bash here**，输入命令安装hexo：
```
npm install -g hexo
```
注意，在回车之后，可能会出现一行WARN的警告语句，不用管它，什么都不要按，等着。。。

```
npm install hexo --save
```

安装完成后用命令检测：
```
npm -v
```
如果出现版本号就代表安装成功了。

初始化博客空间：
```
hexo init
```
完成后可以看到以下文件：

node_modules：是依赖包

public：存放的是生成的页面

scaffolds：命令生成文章等的模板

source：用命令创建的各种文章

themes：主题

_config.yml：整个博客的配置

db.json：source解析所得到的

package.json：项目所需模块项目的配置信息

至此博客空间创建完成。
## 注册github账号
[注册地址](https://github.com/)，用户名、密码和邮箱要记清楚哦。
注册账号后在桌面上右键git bash here，添加刚注册的github的用户名和邮箱：

```
git config --global user.name "CdqPT"
git config --global user.email "123456789@qq.com"
```
## 新建仓库


## 填写信息
无论Owner是大写还是小写，Repository name必须是和Owner名字一样，且必须是**小写**，且后边必须是**.github.io** 

![image.png](https://upload-images.jianshu.io/upload_images/16115686-eaad95efd51a4417.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![image.png](https://upload-images.jianshu.io/upload_images/16115686-368ab2f13fe24fb5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 复制HTTPS链接
回到<>code页，复制HTTPS链接。

![image.png](https://upload-images.jianshu.io/upload_images/16115686-190f64e89ea0a548.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
## 链接hexo与github
在第三步创建的GitRep文件夹下打开_config.yml，推荐使用sumlimb Text3软件。
按照下图修改文件，repository填写刚才复制的链接。
![image.png](https://upload-images.jianshu.io/upload_images/16115686-522de39c94756968.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](https://upload-images.jianshu.io/upload_images/16115686-ba068deb3eeaa513.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 上传博客文件
部署hexo
```
npm install hexo-deployer-git --save    
```
生成本地文件
```
hexo g
```
上传到github
```
hexo d
```

现在，试试在浏览器的地址栏输入：“你的用户名.github.io”，此时，你应该会看到这样的界面：
![image.png](https://upload-images.jianshu.io/upload_images/16115686-fa64d2b6696dd7b6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
## 换主题
刚才的主题很丑吧，选主题要慎重啊，不然后期修改起来有些麻烦，所以还是耐心选好，目前资料比较多的是[next主题](https://theme-next.org/)，功能最全面，问答量最多。也可以自己挑选喜欢的主题：[https://hexo.io/themes/](https://hexo.io/themes/)
可以使用git bash工具git clone 主题链接或者下载zip后解压到themes文件夹内，并改成一个好认的名字比如ocean。然后打开根目录下的_config.yml,修改想要启用的主题名称。
![image.png](https://upload-images.jianshu.io/upload_images/16115686-6cf614ca75497f8b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
接着：

本地部署
```
hexo g
```
生成本地预览
```
hexo s
```
打开浏览器，输入网址预览
[http://localhost:4000/](http://localhost:4000/)
如果满意就上传
```
hexo d
```
## 写博客
博客是用markdown写的，需要学习一些基本语法，可参考:[个人博客-markdown语法笔记](https://purethought.cn/2019/02/15/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2-markdown%E8%AF%AD%E6%B3%95%E7%AC%94%E8%AE%B0/)

然后可以使用简书的实时预览进行写作，效果如下：


![image.png](https://upload-images.jianshu.io/upload_images/16115686-b268f365130f3fe7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


写完后保存成.md格式放在source文件夹下的_post文件夹里。
然后
```
hexo g
```
```
hexo s
```
查看满意后上传，就可以在网站上看到了。
```
hexo d
```
其他写作技巧可参考:[博客-使用jekyll主题创建](https://purethought.cn/2019/02/04/%E5%8D%9A%E5%AE%A2-%E4%BD%BF%E7%94%A8jekyll%E4%B8%BB%E9%A2%98%E5%88%9B%E5%BB%BA/)的第四部分和第五部分。
## 绑定域名
可参考：[博客-使用jekyll主题创建](https://purethought.cn/2019/02/04/%E5%8D%9A%E5%AE%A2-%E4%BD%BF%E7%94%A8jekyll%E4%B8%BB%E9%A2%98%E5%88%9B%E5%BB%BA/)的第三部分。

# 二、ubuntu下安装hexo
## 安装Nodejs 6.16.0
```
curl -sL [https://deb.nodesource.com/setup_6.x](https://deb.nodesource.com/setup_6.x) | sudo -E bash -
```
```
sudo apt-get install -y nodejs
```
## 安装npm
```
sudo apt-get install npm
```
## 安装hexo
```
sudo npm install hexo-cli -g
```
## 设置git
```
git config --global user.email "you@example.com"
git config --global user.name "Your Name"
```
## 设置SSH秘钥
验证有没有SSH秘钥
```
less ~/.ssh/id_rsa.pub
```
如果没有秘钥，添加秘钥
```
ssh-keygen -t rsa -C example@163.com
```
三次回车
查看秘钥
```
less ~/.ssh/id_rsa.pub
```
复制秘钥
到github的setting-SSH and GPG keys中添加刚刚复制的秘钥
OK！

# 三、在MACOS下安装
## 首先检查时候安装了git和node.js，终端输入一下命令，
node -v #是否出现安装版本信息，出现说明已经安装了
git --version #同上述情况
## 如果没有安装，则进行安装,都可以通过直接下载安装测序进行安装，这里不演示，提供下载网址：
[git]: https://sourceforge.net/projects/git-osx-installer/
[node.js]: https://nodejs.org/en/
如果已经安装好了上述的软件，那么可以安装hexo，然后等待安装成功即可。
```
sudo npm install -g hexo-cli
```
## 设置git
```
git config --global user.email "you@example.com"
git config --global user.name "Your Name"
```
## 设置SSH秘钥
验证有没有SSH秘钥
```
less ~/.ssh/id_rsa.pub
```
如果没有秘钥，添加秘钥
```
ssh-keygen -t rsa -C example@163.com
```
三次回车
查看秘钥
```
less ~/.ssh/id_rsa.pub
```
复制秘钥
到github的setting-SSH and GPG keys中添加刚刚复制的秘钥
OK！


参考链接：

1. [https://www.cnblogs.com/visugar/p/6821777.html](https://www.cnblogs.com/visugar/p/6821777.html)
2. [https://www.cnblogs.com/zhaoyu1995/p/6239950.html](https://www.cnblogs.com/zhaoyu1995/p/6239950.html)

