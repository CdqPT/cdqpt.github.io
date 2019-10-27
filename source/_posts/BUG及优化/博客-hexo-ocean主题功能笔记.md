
---
title: 博客-hexo-ocean主题功能笔记
date: 2019-02-15
categories:
- 优化
tags:
- 博客
toc: true
img: /medias/paperimg/bug.jpg
---

改造的东西太多了，防止忘记，特此记录。<!-- more -->


# 一、增加置顶功能

参考链接：[博客-hexo-ocean主题功能笔记](https://purethought.cn/2019/02/10/%E5%8D%9A%E5%AE%A2-hexo%E5%A2%9E%E5%8A%A0%E6%96%87%E7%AB%A0%E7%BD%AE%E9%A1%B6%E5%8A%9F%E8%83%BD/)

效果图：
![置顶功能](https://ws4.sinaimg.cn/large/006jQClrly1g0ytc55e68j30pf07eglv.jpg)
# 二、调整首页边距
参考链接：[https://purethought.cn/2019/02/12/%E4%BF%AE%E6%94%B9%E9%A6%96%E9%A1%B5%E6%96%87%E7%AB%A0%E9%97%B4%E8%B7%9D/](https://purethought.cn/2019/02/12/%E4%BF%AE%E6%94%B9%E9%A6%96%E9%A1%B5%E6%96%87%E7%AB%A0%E9%97%B4%E8%B7%9D/)
效果图：
![首页边距](https://ws1.sinaimg.cn/large/006jQClrly1g0ytdbcpgaj30pq0b7wff.jpg)

# 三、修改页脚
找到theams/ocean/layout/_patial/footer.ejs文件，修改版权信息，增加不蒜子统计功能。
```   js
<footer class="footer">
  <% if (theme.sidebar === 'bottom'){ %>
    <%- partial('_partial/sidebar') %>
  <% } %>
  <div class="outer">
    <ul align="center" class="list-inline">



      

      <li>&copy; <%= date(new Date(), 'YYYY') %> <%= config.title %></li>
      <li><%= __('Author: ') %> <a href="https://purethought.cn/">陈德强</a></li>
      <li><%= __('Email：chendeqiangsp@gmail.com') %> </li>
      
<br />
      <script async src="//busuanzi.ibruce.info/busuanzi/2.3/busuanzi.pure.mini.js">
</script>

<span id="busuanzi_container_site_pv">
    本站总访问量<span id="busuanzi_value_site_pv"></span>次
</span>&nbsp&nbsp&nbsp
<span id="busuanzi_container_site_uv">
  本站访客数<span id="busuanzi_value_site_uv"></span>人次
</span>


      <!--
      <li><a href="/"><%= config.author %></a></li>
      -->
    </ul>
  </div>
</footer>
```

效果图：
![修改页脚](https://ws1.sinaimg.cn/large/006jQClrly1g0yteaoi0tj30ks02pmxc.jpg)

# 四、修改代码行配色
找到theams/ocean/source/css/_partial/highlight.styl文件，修改代码配色及圆角。
```
highlight-background = #F4F4F4
highlight-current-line = #efefef
highlight-selection = #d6d6d6
highlight-foreground = #000080
highlight-comment = #696969
highlight-red = #CC0000
highlight-orange = #CC6633
highlight-yellow = #ffcc66
highlight-green = #009933
highlight-aqua = #66cccc
highlight-blue = #000099
highlight-purple = #993399

$code-block
  background highlight-background
  padding 1.5rem
  margin 1.5rem 0
  border-radius 0.7rem
  overflow auto
  color highlight-foreground
```

效果图：
![代码配色](https://ws1.sinaimg.cn/large/006jQClrly1g0ytfys2i2j30qy0lvgol.jpg)

# 五、阿里云OSS对象存储

视频和图片放在github里会使访问速度超级慢，由于我的主页直接就是一个视频，所以每次都要读取4-5秒，起初也没在意，后来用过一次七牛云，秒开是什么感受！

网上很多人都建议使用七牛云作为存储图片的图床，因为其提供免费空间。我用了一天之后，发现有各种弊端，且不说访问量会受限制，就那个域名整不明白为啥非要备案，搞了老半天心灰意冷，最终决定使用阿里云。

选用阿里云对象存储作为图床具有以下优势：
1. 使用阿里云作为图床便于和域名统一管理；
2. 阿里云对象存储一年才9元！
3. 阿里云访问不首受限制；
4. 大厂不容易倒，稳定且速度快；
5. 不用绑定域名和备案；
6. 允许批量上传。

-----------------------------
## 5.1新建Bucket
如图新建Bucket，然后上传图片或视频，点击右面的**更多**可获取URL链接。

效果图：
![image.png](https://upload-images.jianshu.io/upload_images/16115686-1925d01ba806937e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 5.2 主题修改

**修改首页动画ocean.mp4的获取路径，改为阿里云的外链链接，提升访问速度。**

效果图：
![image.png](https://upload-images.jianshu.io/upload_images/16115686-9fb5c38867423b0d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**修改头像hexo.png的获取路径，改为阿里云的外链链接，提升访问速度。**

效果图：
![image.png](https://upload-images.jianshu.io/upload_images/16115686-1547ee71b25edfb9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 六、添加目录

## 6.1 添加在文章内显示目录
打开themes/ocean/layout/_partial/article.ejs文件，在最开始添加如下代码：
```xml
<!-- 目录内容 -->
        <% if (!index && post.toc){ %>
            
            <p class="show-toc-btn" id="show-toc-btn" onclick="showToc();" style="display:block">
            <span class="btn-bg"></span>
            <span class="btn-text">文章导航</span>
            </p>

            <div id="toc-article" class="toc-article" style="display:none">
                <span id="toc-close" class="toc-close" title="隐藏导航" onclick="showBtn();">×</span>
                <strong class="toc-title">文章目录</strong>

                <%- toc(post.content, {list_number: false}) %>
           </div>

           <script type="text/javascript">
            function showToc(){
                var toc_article = document.getElementById("toc-article");
                var show_toc_btn = document.getElementById("show-toc-btn");
                toc_article.setAttribute("style","display:block");
                show_toc_btn.setAttribute("style","display:none");
                };
            function showBtn(){
                var toc_article = document.getElementById("toc-article");
                var show_toc_btn = document.getElementById("show-toc-btn");
                toc_article.setAttribute("style","display:none");
                show_toc_btn.setAttribute("style","display:block");
                };

           </script>

           
        <% } %>     
        <!-- 目录内容结束 -->
```

## 6.2 添加CSS样式文件
在themes/ocean/source/css/_partial文件夹内新建toc.css文件，添加代码如下：
```css
/*当屏幕大于800像素时 */
@media screen and (min-width: 800px) {

/*目录样式表,电脑端显示按钮在左侧上部 */
#container .show-toc-btn,#container .toc-article{display:block}
.toc-article{z-index:100;background:#fff;border:1px solid #ccc;
  max-width:300px;min-width:200px;max-height:500px;
  overflow-y:auto;-webkit-box-shadow:5px 5px 2px #ccc;box-shadow:5px 5px 2px #ccc;font-size:12px;padding:10px;position:fixed;
  left:0%;top:10%}.toc-article .toc-close{font-weight:700;font-size:20px;cursor:pointer;float:right;color:#ccc}.toc-article .toc-close:hover{color:#000}.toc-article 
.toc{font-size:12px;padding:0;line-height:20px}.toc-article .toc .toc-number{color:#333}.toc-article .toc .toc-text:hover{text-decoration:underline;color:#2a6496}.toc-article li{list-style-type:none}.toc-article .toc-level-1{margin:4px 0}.toc-article 
.toc-child{margin-left:-20px}
@-moz-keyframes cd-bounce-1{0%{opacity:0;-o-transform:scale(1);-webkit-transform:scale(1);-moz-transform:scale(1);-ms-transform:scale(1);transform:scale(1)}60%{opacity:1;-o-transform:scale(1.01);-webkit-transform:scale(1.01);-moz-transform:scale(1.01);-ms-transform:scale(1.01);transform:scale(1.01)}100%{-o-transform:scale(1);-webkit-transform:scale(1);-moz-transform:scale(1);-ms-transform:scale(1);transform:scale(1)}}@-webkit-keyframes cd-bounce-1{0%{opacity:0;-o-transform:scale(1);-webkit-transform:scale(1);-moz-transform:scale(1);-ms-transform:scale(1);transform:scale(1)}60%{opacity:1;-o-transform:scale(1.01);-webkit-transform:scale(1.01);-moz-transform:scale(1.01);-ms-transform:scale(1.01);transform:scale(1.01)}100%{-o-transform:scale(1);-webkit-transform:scale(1);-moz-transform:scale(1);-ms-transform:scale(1);transform:scale(1)}}@-o-keyframes cd-bounce-1{0%{opacity:0;-o-transform:scale(1);-webkit-transform:scale(1);-moz-transform:scale(1);-ms-transform:scale(1);transform:scale(1)}60%{opacity:1;-o-transform:scale(1.01);-webkit-transform:scale(1.01);-moz-transform:scale(1.01);-ms-transform:scale(1.01);transform:scale(1.01)}100%{-o-transform:scale(1);-webkit-transform:scale(1);-moz-transform:scale(1);-ms-transform:scale(1);transform:scale(1)}}@keyframes cd-bounce-1{0%{opacity:0;-o-transform:scale(1);-webkit-transform:scale(1);-moz-transform:scale(1);-ms-transform:scale(1);transform:scale(1)}60%{opacity:1;-o-transform:scale(1.01);-webkit-transform:scale(1.01);-moz-transform:scale(1.01);-ms-transform:scale(1.01);transform:scale(1.01)}100%{-o-transform:scale(1);-webkit-transform:scale(1);-moz-transform:scale(1);-ms-transform:scale(1);transform:scale(1)}
  }
.show-toc-btn{display:none;z-index:10;width:30px;min-height:14px;overflow:hidden;padding:4px 6px 8px 5px;border:1px solid #ddd;border-right:none;position:fixed;
    left:0%;top:10%;text-align:center;background-color:#f9f9f9}.show-toc-btn .btn-bg{margin-top:2px;display:block;width:16px;height:14px;
      background:url(http://blogcdq.oss-cn-beijing.aliyuncs.com/image/blog/show.png) no-repeat;-webkit-background-size:100%;-moz-background-size:100%;background-size:100%}.show-toc-btn .btn-text{color:#999;font-size:12px}.show-toc-btn:hover{cursor:pointer}.show-toc-btn:hover .btn-bg{background-position:0 -16px}.show-toc-btn:hover .btn-text{font-size:12px;color:#ea8010}

.toc-article li ol, .toc-article li ul {
    margin-left: 30px;
}
.toc-article ol, .toc-article ul {
    margin: 10px 0;
}

}



/*当屏幕小于800像素时 */
@media screen and (max-width: 800px) {

/*目录样式表,手机端显示按钮在左下角 */
#container .show-toc-btn,#container .toc-article{display:block}
.toc-article{z-index:100;background:#fff;border:1px solid #ccc;
  max-width:300px;min-width:200px;max-height:500px;
  overflow-y:auto;-webkit-box-shadow:5px 5px 2px #ccc;box-shadow:5px 5px 2px #ccc;font-size:12px;padding:10px;position:fixed;
  left:0%;top:10%}.toc-article .toc-close{font-weight:700;font-size:20px;cursor:pointer;float:right;color:#ccc}.toc-article .toc-close:hover{color:#000}.toc-article 
.toc{font-size:12px;padding:0;line-height:20px}.toc-article .toc .toc-number{color:#333}.toc-article .toc .toc-text:hover{text-decoration:underline;color:#2a6496}.toc-article li{list-style-type:none}.toc-article .toc-level-1{margin:4px 0}.toc-article 
.toc-child{margin-left:-20px}
@-moz-keyframes cd-bounce-1{0%{opacity:0;-o-transform:scale(1);-webkit-transform:scale(1);-moz-transform:scale(1);-ms-transform:scale(1);transform:scale(1)}60%{opacity:1;-o-transform:scale(1.01);-webkit-transform:scale(1.01);-moz-transform:scale(1.01);-ms-transform:scale(1.01);transform:scale(1.01)}100%{-o-transform:scale(1);-webkit-transform:scale(1);-moz-transform:scale(1);-ms-transform:scale(1);transform:scale(1)}}@-webkit-keyframes cd-bounce-1{0%{opacity:0;-o-transform:scale(1);-webkit-transform:scale(1);-moz-transform:scale(1);-ms-transform:scale(1);transform:scale(1)}60%{opacity:1;-o-transform:scale(1.01);-webkit-transform:scale(1.01);-moz-transform:scale(1.01);-ms-transform:scale(1.01);transform:scale(1.01)}100%{-o-transform:scale(1);-webkit-transform:scale(1);-moz-transform:scale(1);-ms-transform:scale(1);transform:scale(1)}}@-o-keyframes cd-bounce-1{0%{opacity:0;-o-transform:scale(1);-webkit-transform:scale(1);-moz-transform:scale(1);-ms-transform:scale(1);transform:scale(1)}60%{opacity:1;-o-transform:scale(1.01);-webkit-transform:scale(1.01);-moz-transform:scale(1.01);-ms-transform:scale(1.01);transform:scale(1.01)}100%{-o-transform:scale(1);-webkit-transform:scale(1);-moz-transform:scale(1);-ms-transform:scale(1);transform:scale(1)}}@keyframes cd-bounce-1{0%{opacity:0;-o-transform:scale(1);-webkit-transform:scale(1);-moz-transform:scale(1);-ms-transform:scale(1);transform:scale(1)}60%{opacity:1;-o-transform:scale(1.01);-webkit-transform:scale(1.01);-moz-transform:scale(1.01);-ms-transform:scale(1.01);transform:scale(1.01)}100%{-o-transform:scale(1);-webkit-transform:scale(1);-moz-transform:scale(1);-ms-transform:scale(1);transform:scale(1)}
  }
.show-toc-btn{display:none;z-index:10;width:30px;min-height:14px;overflow:hidden;padding:4px 6px 8px 5px;border:1px solid #ddd;border-right:none;position:fixed;
    left:0%;bottom:0%;text-align:center;background-color:#f9f9f9}.show-toc-btn .btn-bg{margin-top:2px;display:block;width:16px;height:14px;
      background:url(http://blogcdq.oss-cn-beijing.aliyuncs.com/image/blog/show.png) no-repeat;-webkit-background-size:100%;-moz-background-size:100%;background-size:100%}.show-toc-btn .btn-text{color:#999;font-size:12px}.show-toc-btn:hover{cursor:pointer}.show-toc-btn:hover .btn-bg{background-position:0 -16px}.show-toc-btn:hover .btn-text{font-size:12px;color:#ea8010}

.toc-article li ol, .toc-article li ul {
    margin-left: 30px;
}
.toc-article ol, .toc-article ul {
    margin: 10px 0;
}

}
```

## 6.3 添加CSS样式路径
在themes/ocean/source/css/style.styl中加入**@import '_partial/toc.css'**，如下图所示：
```
@import "_variables"
@import "_mixins"
@import "_feathericon"
@import "_normalize"
@import '_partial/toc.css'
```
## 6.4 效果图
目录大小会自动调整，很棒。

![展开目录](https://upload-images.jianshu.io/upload_images/16115686-779f0430c76763dd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![收缩目录](https://ws1.sinaimg.cn/large/006jQClrly1g0ythg3o4uj303g07jmwy.jpg)

# 七、添加封面图

写markdown时在head里加入如下形式：

albums: [
        [URL1,name1],
        [URL2,name2]
        ]
例如：
```
---
title: 高效电子化办公方案
date: 2019-02-04
categories:
- 优化
tags:
- 博客
top: true
toc: true
albums: [
        [http://blogcdq.oss-cn-beijing.aliyuncs.com/image/gallery/zhuhaistudy.jpg,暨南大学珠海校区]
        ]

---
```

![封面图](https://ws4.sinaimg.cn/large/006jQClrly1g0ytirwa2ij30qj0dbn3u.jpg)

# 八、添加相册
## 8.1 新建标签页
```
hexo new page gallery
```
## 8.2 添加相片
打开仓库根目录/source/gallery/index.md文件，在head部分添加：

albums: [
        [URL1,name1],
        [URL2,name2]
        ]

例如：
```
---
title: 相册
date: 2019-02-04 21:35:22
albums: [
        [http://blogcdq.oss-cn-beijing.aliyuncs.com/image/gallery/HKmotianlun.jpg,香港游],
        [http://blogcdq.oss-cn-beijing.aliyuncs.com/image/gallery/IMUTEvening%20Party.jpg,毕业晚会],
        [http://blogcdq.oss-cn-beijing.aliyuncs.com/image/gallery/IMUTsky.jpg,IMUT],
        [http://blogcdq.oss-cn-beijing.aliyuncs.com/image/gallery/XiaMenGulangyu.jpg,厦门鼓浪屿],
        [http://blogcdq.oss-cn-beijing.aliyuncs.com/image/gallery/atZhuHai.jpg,珠海暨大]
        ]
---
```

# 九、添加分类页
## 9.1 新建分类页
```
hexo new page tags
```
## 10.2 添加分类页头信息
打开根目录/source/categories/index.md，增添layout: "categories"和type: "categories"，如下所示：
```
---
title: 分类
date: 2019-02-04 17:44:45
type: "categories"
layout: "categories"
comments: false
---
```
## 9.3 添加分类页样式
创建themes/ocean/layout/categories.ejs文件，添加内容如下：
```js
<article class="article article-type-post show">
  <header class="article-header">
  <h1 align="center" class="article-title" itemprop="name">
   
    <%= page.title %>
  </h1>
  </header>

  <% if (site.categories.length){ %>
  <div class="category-all-page article-type-post show">
    <h3 align="right">Total:&nbsp;<%= site.categories.length %>&nbsp;Categories </h3>

    <ul  align="center" style="font-size:18px" class="category-list">
    
    <% site.categories.sort('name').each(function(item){ %>
      <% if(item.posts.length){ %>

          <a href="<%- config.root %><%- item.path %>" title="<%= item.name %>"><%= item.name %><sup>[<%= item.posts.length %>]</sup></a>
       &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
      <% } %>
    <% }); %>
    </ul>
  </div>
  <% } %>
</article>
```
## 9.4 添加分类页侧边栏显示
在根目录/_config.yml文件中添加分类页。
```
# Menu
menu:
  首页: /
  归档: /archives
  分类: /categories 
  标签云: /tags
  相册: /gallery
  关于: /about
rss: /atom.xml
```

## 9.5 添加分类页图标
打开themes/ocean/source / css / _partial / navbar.styl，增添标签云图标。标签云图标采用 [feathericon](https://feathericon.com/)图标库。
```
 .nav-item
        &:nth-child(1)         // home
          .nav-item-link
            &::before
              content '\f12f'
        &:nth-child(2)         // archives
          .nav-item-link
            &::before
              content '\f12a'
        &:nth-child(3)         // 分类
          .nav-item-link
            &::before
              content '\f198'

        &:nth-child(4)         // 标签云
          .nav-item-link
            &::before
              content '\f115'

        &:nth-child(5)         // gallery
          .nav-item-link
            &::before
              content '\f1a9'
        &:nth-child(6)         // about
          .nav-item-link
            &::before
              content '\f174'
```

![分类页](https://wx2.sinaimg.cn/large/006jQClrly1g0ytjttxkhj30tq06kweq.jpg)

# 十、添加标签云
## 10.1 创建标签页
```
hexo new page tags
```
## 10.2 添加标签云头信息
打开根目录/source/tags/index.md，增添layout: "tags"和type: "tags"，如下所示：
```HTML
---
title: 标签云
date: 2019-02-17 09:38:28
layout: "tags"
type: "tags"
---

```
## 10.3 添加标签云样式

创建themes/ocean/layout/tags.ejs文件，添加内容如下：
```
<article class="article article-type-post show">
  <header class="article-header">
  <h1 align="center" class="article-title" itemprop="name">
   
    <%= page.title %>(<%= site.tags.length %>)
  </h1>
  </header>


<% if (page.path === "tags/index.html"){ %>
<hr>

    <br/>
    
    
    <div class="tags">
        <%- tagcloud({
            min_font: 16, 
            max_font: 35, 
            amount: 999, 
            color: true, 
            start_color: 'gray', 
            end_color: 'black',
        }) %>
    </div>
    <style>
        .category-list li{
            display: inline-block;
            margin: 0 1em .5em 0;
            padding: 4px;
            border: 1px solid lightgray;
            font-size: 1.2em;
        }
        .category-list a {
            color: gray;
        }
        .category-list-item:hover a {
            color: gray;
            text-decoration: none;
            font-weight: bold;
        }
        .category-list-count {
            margin-left: 2px;
            font-size: .9em;
        }
        .article-entry ul li:before{
            display: none;
        }
        .article-inner  {
            text-align: center;
        }
        .tags {
            max-width: 40em;
            margin: 2em auto;
            margin-top: 0em;
        }
        .tags a {
            margin-right: 1em;
            border-bottom: 1px solid gray;
            line-height: 65px;
            white-space: nowrap;
        }
        .tags a:hover {
            border-bottom: 2px solid black;
            font-style: italic;
            text-decoration: none;
        }
    </style>
<% } %>

</article>
```
## 10.4 添加标签云侧边栏显示
在根目录/_config.yml文件中添加标签云。
```
# Menu
menu:
  首页: /
  归档: /archives
  分类: /categories 
  标签云: /tags
  相册: /gallery
  关于: /about
rss: /atom.xml
```

## 10.5 添加标签云图标
打开themes/ocean/source / css / _partial / navbar.styl，增添标签云图标。标签云图标采用 [feathericon](https://feathericon.com/)图标库。
```
 .nav-item
        &:nth-child(1)         // home
          .nav-item-link
            &::before
              content '\f12f'
        &:nth-child(2)         // archives
          .nav-item-link
            &::before
              content '\f12a'
        &:nth-child(3)         // 分类
          .nav-item-link
            &::before
              content '\f198'

        &:nth-child(4)         // 标签云
          .nav-item-link
            &::before
              content '\f115'

        &:nth-child(5)         // gallery
          .nav-item-link
            &::before
              content '\f1a9'
        &:nth-child(6)         // about
          .nav-item-link
            &::before
              content '\f174'
```

![标签云](https://wx1.sinaimg.cn/large/006jQClrly1g0ytkl6ohcj30sm08rdgk.jpg)

# 十一、修改行内代码样式
找到themes/ocean/source/css/_partial/hightlight.styl文件，修改此部分代码：
```
.article-entry
  pre, code
    font-family inherit
    text-shadow 1px 1px 0 rgba(0, 0, 0, 0.3)
  code
    background  white
    color:    #FF8C00;
    font-weight:light;
    padding .25rem
    border-radius(.5rem)
    font-family: "微软雅黑" , "宋体" , "黑体" ,Arial;
```

# 十二、修改引用样式
找到themes/ocean/source/css/_extend.styl文件，修改代码如下：
```
  blockquote
    border-style: solid;
    border-top-width: 0px;
    border-right-width: 0px;
    border-bottom-width: 0px;
    border-left-width: 4px;
    border-color: #3f7aaa;
    border-radius: 0px;
    color: #000;
    font-family: "微软雅黑" , "宋体" , "黑体" ,Arial;
    font-size: 14px;
    font-weight: bold;
    height: 18px;
    line-height: 18px;
    margin: 15px 0 15px 0 !important;
    padding: 0 0 0 8px;
    text-shadow: 1px 1px 0 rgba(0, 0, 0, 0.3);



    > :first-child
      margin-top 0;
    > :last-child
      margin-bottom 0;
    footer
      cite
        &:before
          content "—"
          padding 0 .5rem
```

# 十三、修改正文字体样式
找到themes/ocean/source/css/_extend.styl文件，修改第42行代码如下：
```
  table
    margin 1.5rem 0
  p
    font-size 17px 
    color black
```