
---
title: python-监测网站并发送邮件
date: 2019-04-07 21:22:00
categories: 笔记
tags: python
toc: true
img: /medias/paperimg/python.jpg
summary: 网站更新过快时会有一点监测延时。
author: 陈德强
top: 
password: 
cover: 
---




```python


#coding=utf-8

##########################爬取网页####################################
import urllib.request
from bs4 import BeautifulSoup

#把所有需要采集的页面地址都放入url_list这个列表，用逗号隔开
url_list=['https://purethought.cn/']

def get_webInfo(url,option):
    req=urllib.request.Request(url)
    rsp=urllib.request.urlopen(req)
    html=rsp.read().decode('utf-8','ignore')
    html=BeautifulSoup(html,'html.parser')
    #此处limit改成需要去除掉的其他网址数量
    for link in html.find_all('a',limit=30):
        info_link=link.get('href')
        info_text=link.get_text(strip=True)
    option=option
    if option==0:
        return '标题：'+info_text+';'+'链接：'+'https://purethought.cn'+info_link
    if option==1:
        return info_text
    if option==2:
        return 'https://purethought.cn'+info_link
#option选项，0是全部，1是标题，2是链接
def parseWeb(url_list,option):
    result = []
    for url in url_list:
        option=option
        webInfo = get_webInfo(url,option)
        result.append(webInfo)
    return result


###########################发邮件####################
from email.mime.text import MIMEText
from email.header import Header
from smtplib import SMTP_SSL
#title修改为邮件主题
title = '网站信息更新通知'
#receiver修改为接收者邮箱
receiver = '2926820576@qq.com'
def send_mail(title,receiver):
    #qq邮箱smtp服务器
    host_server = 'smtp.qq.com'
    #sender_qq为发件人的qq号码
    sender_qq = '928277452@qq.com'
    #pwd为qq邮箱的授权码，在邮箱-设置-账户中设置
    pwd = '？？？' 
    #发件人的邮箱
    sender_qq_mail = '928277452@qq.com'
    #收件人邮箱
    receiver = receiver
    #邮件的正文内容
    content_1=str(parseWeb(url_list,1))
    content_1=content_1[2:-2]
    content_2=str(parseWeb(url_list,2))
    content_2=content_2[2:-2]
    content = '<h3 >您关注的网站已更新:</h3>'+'<strong style="color: green;">标题：</strong>'+content_1+'</br><strong style="color: green;">链接：</strong>'+content_2

    mail_content = content
    mail_title = title
    smtp = SMTP_SSL(host_server)
    smtp.set_debuglevel(1)
    smtp.ehlo(host_server)
    smtp.login(sender_qq, pwd)
    #msg = MIMEText(mail_content, "plain", 'utf-8')
    msg = MIMEText(mail_content, "html", 'utf-8')
    msg["Subject"] = Header(mail_title, 'utf-8')
    msg["From"] = sender_qq_mail
    msg["To"] = receiver
    smtp.sendmail(sender_qq_mail, receiver, msg.as_string())
    smtp.quit()



#########################################比较###############################

#定义一个字典，包含一个空元素history，字典可以储存任何形式的值
tmp = {'history':None}


def check():
    if (tmp['history']):
        history = tmp['history']
        now = parseWeb(url_list,0)
        if (history == now):
            print('未发现更新！')
        else:
            print('发现更新！！！！！！！！！！！')
            print('----------------------------------------------------------------')
            send_mail(title,receiver)
            print('更新内容如下:'+'\n',parseWeb(url_list,0))
            print('----------------------------------------------------------------')
        tmp ['history'] = now
    else:
        tmp['history'] = parseWeb(url_list,0)
        print('最新内容如下:'+'\n',parseWeb(url_list,0))
        
        
#########################################主循环###############################
import time
print('更新速度：10s')
while True:
    print (time.strftime('%Y-%m-%d %H:%M:%S',time.localtime(time.time())))
    check()
    time.sleep(9)


```

参考链接：[Python发送邮件(最全)](https://www.jianshu.com/p/abb2d6e91c1f)