---
title: python-爬取网站信息
date: 2019-04-07 13:04:00
categories: 笔记
tags: python
toc: true
img: /medias/paperimg/python.jpg
summary: python-爬取网站信息
author: 陈德强
top: 
password: 
cover: 
---


获取中铁兰州局 销售-项目公告-中标公告等页面的第一条信息。

```python
#coding=utf-8
#导入urllib.request模块
import urllib.request
#导入BeautifulSoup模块
from bs4 import BeautifulSoup

#把所有需要采集的页面地址都放入url_list这个列表
url_list=['http://wz.lanzh.95306.cn/mainPageNoticeList.do?method=init&id=2000001&cur=1','http://wz.lanzh.95306.cn/mainPageNoticeList.do?method=init&id=2200001&cur=1','http://wz.cd-rail.com/mainPageNoticeList.do?method=init&id=2000001&cur=1','http://wz.cd-rail.com/mainPageNoticeList.do?method=init&id=2200001&cur=1','http://wz.xian.95306.cn/mainPageNoticeList.do?method=init&id=2000001&cur=1','http://wz.xian.95306.cn/mainPageNoticeList.do?method=init&id=2200001&cur=1','http://wz.huhht.95306.cn/mainPageNoticeList.do?method=init&id=2000001&cur=1','http://wz.huhht.95306.cn/mainPageNoticeList.do?method=init&id=2200001&cur=1','http://wz.qingz.95306.cn/mainPageNoticeList.do?method=init&id=2000001&cur=1','http://wz.qingz.95306.cn/mainPageNoticeList.do?method=init&id=2200001&cur=1']

#第一层循环，把url都导出来
for url in url_list:
    #定义发送的请求
    req=urllib.request.Request(url)
    #将服务器返回的页面放入rsp变量
    rsp=urllib.request.urlopen(req)
    #读取这个页面，并解码成utf-8格式，忽略错误,放入变量html中
    html=rsp.read().decode('utf-8','ignore')
    #使用BeautifulSoup模块解析变量中的web内容
    html=BeautifulSoup(html,'html.parser')
    #第二层循环，找出所有的a标签，并赋值给变量 link
    for link in html.find_all('a',limit=3):
        #把href中的内容赋值给info_link
        info_link=link.get('href')
        #把a标签中的文字赋值给info_text,并去除空格
        info_text=link.get_text(strip=True)
    #打印出info_text和info_link，并换行
    print(info_text)
    print(url[:-50]+info_link+'\n')
```

获取结果如下：
```
[线下][竞卖]4月9日废旧物资网上竞价销售公告
http://wz.lanzh.95306.cn/mainPageNotice.do?method=info&id=IJ002019032701884428%40IJ002019032701884429%4030

[线下][竞卖]关于兰州车站场地租赁到期招商公告项目的结果公示
http://wz.lanzh.95306.cn/mainPageNotice.do?method=info&id=OJ002019011201008185%40OJ002018121201007992%403204

[线下][竞卖]中国铁路成都局集团有限公司贵阳机务段2019年5-10月份报废物资（废蓄电池、废混合油、废油桶）谈判销售公告
http://wz.cd-rail.com/mainPageNotice.do?method=info&id=IW002019040401050803%40IW002019040401050804%4030

[线下][竞卖]凯里工务段2月托架销售结果
http://wz.cd-rail.com/mainPageNotice.do?method=info&id=IW002019040301050555%40IW002019021901007905%4032

[正式][竞卖]中国铁路西安局集团有限公司宝鸡电务段关于2019年第四次废旧信号器材网上竞价销售的项目公告
http://wz.xian.95306.cn/mainPageNotice.do?method=info&id=IY002019040306587570%40IY002019040306587558%4030

[正式][竞卖]延安工务段关于2019年延安工务段废旧物资销售公告的成交公告
http://wz.xian.95306.cn/mainPageNotice.do?method=info&id=OY002019032904912544%40IY002019032106586742%4032

[正式][竞卖]呼和浩特铁路局关于HTFJ19-05 废旧物资拍卖的项目公告
http://wz.huhht.95306.cn/mainPageNotice.do?method=info&id=OC002019032700677356%40OC002019032600677232%4030

[正式][竞卖]呼和浩特铁路局关于HTFJ19-05 废旧物资拍卖的成交公告
http://wz.huhht.95306.cn/mainPageNotice.do?method=info&id=OC002019040300684660%40OC002019032600677232%4032

[正式][竞卖]青藏铁路公司关于2019年第02批废机油、废电池网上竞价销售的项目公告
http://wz.qingz.95306.cn/mainPageNotice.do?method=info&id=IO002019021500055664%40IO002019021500055653%4030

[正式][竞卖]青藏铁路公司关于2019年第02批报废车轴、车轮等网上竞价销售的成交公告
http://wz.qingz.95306.cn/mainPageNotice.do?method=info&id=OO002019030100314256%40IO002019021500055646%4032
```



参考自：[使用python3做一个爬虫，监控网站信息上篇（获取网页信息）](https://www.xiaoweigod.com/code/1609.html)