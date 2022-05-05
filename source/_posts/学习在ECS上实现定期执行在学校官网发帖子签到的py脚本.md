---
title: 学习在ECS上实现定期执行在学校官网发帖子签到的py脚本.md
date: 1111-11-11 11:11:11
categories:
  - OldBlog(Before20220505)
tags:
  - OldBlog(Before20220505)
---

## 说明 - 2022-05-05
本篇博客为本人原创, 原发布于CSDN, 在搭建个人博客后使用爬虫批量爬取并挂到个人博客, 出于一些技术原因博客未能完全还原到初始版本(而且我懒得修改), 在观看体验上会有一些瑕疵 ,若有需求会发布重制版总结性新博客。发布时间统一定为1111年11月11日。钦此。

**本文不作为较专业的资料，仅为萌新不专业的试水和踩坑** 

## 1.编写签到脚本

从这个环节开始我就遇到了许多问题，比如可以复制cookie实现签到，但不会获取cooike  
实际上直到现在写博客，我始终没有做到完整地吧所需要的cookie提取出来，对于爬虫也是一知半解

直接上误打误撞写出来的拉跨代码（部分内容隐蔽或更改）：


​    
```python
import requests
import time

url='在学校官网登录请求的url'
header={
        'Referer': 'secret',
        'User-Agent': 'mystery',
        'Content-Type': 'application/x-www-form-urlencoded'
    }
data={
        'IPT_LOGINUSERNAME': 'what?',
        'IPT_LOGINPASSWORD': 'you don\'t know'
    }

session=requests.session()
res=session.post(url,headers=header,data=data)

posturl='发帖子请求的url'
```


​    
```python
headersx={
        'Content-Type': 'application/x-www-form-urlencoded',
        'Referer': 'mytsery',
        'User-Agent': 'jsadsadfsadfasd',
    }

datax={
        'opt': 'toreply',
        'frompage': 'common',
        'forumid':'3743',
        'threadid':'114775',
        'parentid':'239973',
        'type': '0',
        'subject':'帖子标题的一些内容'.encode("gbk"),
        'body': '<p>学号 某某签到</p>'.encode("gbk")#编码格式一定要换，否则发出乱码
        
    }
resx=session.post(posturl,headers=headersx,data=datax)

#相当于一个日志
with open('journal.txt','a') as f:#这是在本地写的，放在服务器上要使用绝对路径，否则会在根目录创建文件
    string=''
    tm=time.localtime()
    for i in range(6):
        string+=str(tm[i])+' '
    else:
        string+='OVER \n'
    f.write(string)
```


​    

## 2.在ECS上令其定时执行

我的云服务器装的乌班图系统

先通过以下操作测试了一下py文件是否能在云服务器上运行，且运行成功

> python3 签到文件

然后通过第一行的操作打开控制定时执行的文件，添加内容，最后两行是我新添加的内容(排版不是很好，服务器上看是很清晰的)。  
关于怎样设置执行时间见传送门：<https://www.runoob.com/linux/linux-comm-crontab.html>

> vi /etc/corntab

> # m h dom mon dow user command  
>  17 * * * * root cd / && run-parts --report /etc/cron.hourly  
>  25 6 * * * root test -x /usr/sbin/anacron || ( cd / && run-parts --report
> /etc/cron.daily )  
>  47 6 * * 7 root test -x /usr/sbin/anacron || ( cd / && run-parts --report
> /etc/cron.weekly )  
>  52 6 1 * * root test -x /usr/sbin/anacron || ( cd / && run-parts --report
> /etc/cron.monthly )  
>  0 14 * * 3 root /usr/bin/python3.6 /home/signIn195C/login195C.py  
>  0 10 * * 5 root /usr/bin/python3.6 /home/signIn195C/login195C.py

## 注意

python脚本若要建立（或写入）文件一定要使用绝对路径。笔者手动执行程序时会在脚本所在的目录建立（或写入）文件，但自动执行时文件会在根目录建立（或写入）

