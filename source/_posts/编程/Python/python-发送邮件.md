
---
title: python-发送邮件
date: 2019-04-06 21:28:00
categories: 笔记
tags: python
toc: true
img: /medias/paperimg/python.jpg
summary: 编写python发送邮件功能
author: 陈德强
top: 
password: 
cover: 
---
# 一、发送纯文本内容

代码示例：
```python
#! /usr/bin/env python
#coding=utf-8

from email.mime.text import MIMEText
from email.header import Header
from smtplib import SMTP_SSL


#qq邮箱smtp服务器
host_server = 'smtp.qq.com'
#sender_qq为发件人的qq号码
sender_qq = '928277452@qq.com'
#pwd为qq邮箱的授权码，在邮箱-设置-账户中设置
pwd = 'balabala' 
#发件人的邮箱
sender_qq_mail = '928277452@qq.com'
#收件人邮箱
receiver = '1052246868@qq.com'

#邮件的正文内容
mail_content = '这是一个测试python发送邮件的程序'
#邮件标题
mail_title = '不要紧张，我是老司机'

#ssl登录
smtp = SMTP_SSL(host_server)
#set_debuglevel()是用来调试的。参数值为1表示开启调试模式，参数值为0关闭调试模式
smtp.set_debuglevel(1)
smtp.ehlo(host_server)
smtp.login(sender_qq, pwd)

msg = MIMEText(mail_content, "plain", 'utf-8')
msg["Subject"] = Header(mail_title, 'utf-8')
msg["From"] = sender_qq_mail
msg["To"] = receiver
smtp.sendmail(sender_qq_mail, receiver, msg.as_string())
smtp.quit()
```

参考链接：[Python发送邮件(最全)](https://www.jianshu.com/p/abb2d6e91c1f)