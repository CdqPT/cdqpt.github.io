---
title: 博客-hexo增加文章置顶功能
date: 2019-02-10
categories:
- 优化
tags:
- 博客
toc: true
img: /medias/paperimg/bug.jpg
---

 增加置顶功能和图标<!-- more -->




# 安装置顶插件

```
npm uninstall hexo-generator-index --save
```

```
npm install hexo-generator-index-pin-top --save
```
# 增加置顶图标

在**themes/ocean/layout/_partial/article.ejs**中找到<div class="article-meta">

修改/增添内容如下：
```
<% if (index || is_post()) { %>
      <div class="article-meta">
      <% if (post.top) { %>
            <i class="fa fa-thumb-tack"></i>
            <font color=7D26CD>置顶</font>
            <span class="post-meta-divider">|</span>
          <% } %>
        <%- partial('post/date', {class_name: 'article-date', date_format: null}) %>
        <%- partial('post/category') %>
      </div>
    <% } %>
```

参考链接：[https://blog.csdn.net/qwerty200696/article/details/79010629](https://blog.csdn.net/qwerty200696/article/details/79010629)