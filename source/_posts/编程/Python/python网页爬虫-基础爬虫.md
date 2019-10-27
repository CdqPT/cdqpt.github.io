---
title: python网页爬虫-基础爬虫
date: 2019-06-12 13:51:00
categories: 笔记
tags: python
toc: true
img: /medias/paperimg/python.jpg
summary: 爬取100个百度百科网络爬虫词条以及相关词条的标题、摘要和链接等信息。
top: 
password: 
cover: 

---

首先这个爬虫项目功能简单，仅仅考虑功能实现，未涉及优化和稳健性的考虑。再者爬虫虽小，五脏俱全，大型爬虫有的基础模块，这个爬虫都有，只不过实现方式、优化方式，大型爬虫做得更加全面、多样。
本次实战项目的需求是爬取100个百度百科网络爬虫词条以及相关词条的标题、摘要和链接等信息。

SpiderMan.py
```python
#coding:utf-8
from URLManager import UrlManager
from HtmlDownloader import HtmlDownloader
from HtmlParser import HtmlParser

from DataOutput import DataOutput


class SpiderMan(object):
    def __init__(self):
        self.manager = UrlManager()
        self.downloader = HtmlDownloader()
        self.parser = HtmlParser()
        self.output = DataOutput()
    def crawl(self,root_url):
        #添加入口URL
        self.manager.add_new_url(root_url)
        #判断url管理器中是否有新的url，同时判断抓取了多少个url
        while(self.manager.has_new_url() and self.manager.old_url_size()<100):
            try:
                #从URL管理器获取新的url
                new_url = self.manager.get_new_url()
                #HTML下载器下载网页
                html = self.downloader.download(new_url)
                #HTML解析器抽取网页数据
                new_urls,data = self.parser.parser(new_url,html)
                #将抽取到url添加到URL管理器中
                self.manager.add_new_urls(new_urls)
                #数据存储器储存文件
                self.output.store_data(data)
                print("已经抓取%s个链接"%self.manager.old_url_size())
            except Exception as e:
                print("crawl failed")
            #数据存储器将文件输出成指定格式
        self.output.output_html()

if __name__=="__main__":
    spider_man = SpiderMan()
    spider_man.crawl("http://baike.baidu.com/view/284853.htm")
```
URLManager.py
```python
#coding:utf-8
## 定义一个新式类,继承自object
class UrlManager(object):
    def __init__(self):
        #__init__()方法是一种特殊的方法，被称为类的构造函数或初始化方法，当创建了这个类的实例时就会调用该方法
        #self 代表类的实例，self 在定义类的方法时是必须有的，虽然在调用时不必传入相应的参数。
        #将new_url和old_urls设置为set()形式，目的是去重，其实这一步也相当于定义两个变量。
        self.new_urls = set()#未爬取URL集合
        self.old_urls = set()#已爬取URL集合

    def has_new_url(self):
        #判断是否有未爬取的URL，获取未爬取URL集合的大小。返回True或False
        return self.new_url_size()!=0

    def get_new_url(self):
        #获取一个未爬取的URL
        new_url = self.new_urls.pop()
        self.old_urls.add(new_url)
        return new_url

    def add_new_url(self,url):
        '''
         将新的URL添加到未爬取的URL集合中
        :param url:单个URL
        :return:
        '''
        if url is None:
            return
        if url not in self.new_urls and url not in self.old_urls:
            self.new_urls.add(url)

    def add_new_urls(self,urls):
        '''
        将新的URLS添加到未爬取的URL集合中
        :param urls:url集合
        :return:
        '''
        if urls is None or len(urls)==0:
            return
        for url in urls:
            self.add_new_url(url)

    def new_url_size(self):
        '''
        获取未爬取URL集合的s大小
        :return:
        '''
        return len(self.new_urls)

    def old_url_size(self):
        '''
        获取已经爬取URL集合的大小
        :return:
        '''
        return len(self.old_urls)
```
HtmlDownloader.py
```python
#coding:utf-8
import requests
class HtmlDownloader(object):

    def download(self,url):
        if url is None:
            return None
        #设置代理，目的是伪装成浏览器，这是一只优秀的爬虫应该有的觉悟。
        user_agent = 'Mozilla/4.0 (compatible; MSIE 5.5; Windows NT)'
        headers={'User-Agent':user_agent}
        r = requests.get(url,headers=headers)
        #转码并返回网页
        if r.status_code==200:
            r.encoding='utf-8'
            return r.text
        return None
```
HtmlParser.py
```python
#coding:utf-8
import re
import urllib.parse
from bs4 import BeautifulSoup


class HtmlParser(object):

    def parser(self,page_url,html_cont):
        '''
        用于解析网页内容抽取URL和数据
        :param page_url: 下载页面的URL
        :param html_cont: 下载的网页内容
        :return:返回URL和数据
        '''
        if page_url is None or html_cont is None:
            return
        soup = BeautifulSoup(html_cont,'html.parser',from_encoding='utf-8')
        new_urls = self._get_new_urls(page_url,soup)
        new_data = self._get_new_data(page_url,soup)
        return new_urls,new_data


    def _get_new_urls(self,page_url,soup):
        '''
        抽取新的URL集合
        :param page_url: 下载页面的URL
        :param soup:soup
        :return: 返回新的URL集合
        '''
        new_urls = set()
        #抽取符合要求的a标签
        #原书代码
        # links = soup.find_all('a',href=re.compile(r'/view/\d+\.htm'))
        #2017-07-03 更新,原因百度词条的链接形式发生改变
        links = soup.find_all('a', href=re.compile(r'/item/.*'))
        for link in links:
            #提取href属性
            new_url = link['href']
            #拼接成完整网址
            new_full_url = urllib.parse.urljoin(page_url,new_url)
            new_urls.add(new_full_url)
        return new_urls
    def _get_new_data(self,page_url,soup):
        '''
        抽取有效数据
        :param page_url:下载页面的URL
        :param soup:
        :return:返回有效数据
        '''
        data={}
        data['url']=page_url
        title = soup.find('dd',class_='lemmaWgt-lemmaTitle-title').find('h1')
        data['title']=title.get_text()
        summary = soup.find('div',class_='lemma-summary')
        #获取到tag中包含的所有文版内容包括子孙tag中的内容,并将结果作为Unicode字符串返回
        data['summary']=summary.get_text()
        return data
```
DataOutput.py
```python
#coding:utf-8
import codecs

class DataOutput(object):

    def __init__(self):
        self.datas=[]
    def store_data(self,data):
        if data is None:
            return
        self.datas.append(data)

    def output_html(self):
        #储存位置
        fout=codecs.open('baike.html','w',encoding='utf-8')
        #储存形式
        fout.write("<html>")
        fout.write("<head><meta charset='utf-8'/></head>")
        fout.write("<body>")
        fout.write("<table>")
        for data in self.datas:
            fout.write("<tr>")
            fout.write("<td>%s</td>"%data['url'])
            fout.write("<td>%s</td>"%data['title'])
            fout.write("<td>%s</td>"%data['summary'])
            fout.write("</tr>")

        fout.write("</table>")
        fout.write("</body>")
        fout.write("</html>")
        fout.close()
```