---
title: 【爬虫】 爬取云班课上老师发的视频资源 python.md
date: 1111-11-11 11:11:11
categories:
  - OldBlog(Before20220505)
tags:
  - OldBlog(Before20220505)
---

## 说明 - 2022-05-05
本篇博客为本人原创, 原发布于CSDN, 在搭建个人博客后使用爬虫批量爬取并挂到个人博客, 出于一些技术原因博客未能完全还原到初始版本(而且我懒得修改), 在观看体验上会有一些瑕疵 ,若有需求会发布重制版总结性新博客。发布时间统一定为1111年11月11日。钦此。

## 蒟蒻声明

这个程序虽然可以差强人意地实现功能但是烂的一批，也不打算做出较大改动了。后面会总结一些不足之处，以警示以后。

## 思路

。  
1.  
通过抓包发现云班课的视频时m3u8的，即一个m3u8的文件中记录了数个ts类型视频的链接，这些ts视频的时常一般为10秒。想爬取某个视频只需要获取视频对应的m3u8文件，依次把m3u8文件中的ts文件下载并通过os.system()调用命令行把一系列的ts文件合并为mp4  
2.  
步骤1是单独下载一个视频的思路，若想下载所有视频，只需要找出所有视频的m3u8，重复步骤1。

## 单独下载一个m3u8

笔者仅仅在要爬的课程中实验该程序可以达到目的，至于其他的m3u8就不清楚了  
1.  
获取m3u8文件后解析其内容，做到可以提取出所有的ts文件链接  
2.  
把所有的ts文件下载本地的同一个文件夹中  
3.  
调用命令行，例如这样：  
os.system(‘copy /b ‘+os.path.abspath(tempworkpath)+’\ _.ts
‘+os.path.abspath(pathOUTPUT)+’\’+nameOUTPUT+’.mp4’)  
合并所有ts文件为一个新的mp4文件  
4.  
把这一次下载的ts文件删掉，以备下一次操作  
os.system('del ‘+os.path.abspath(tempworkpath)+’\_.ts’)

    
    
    import re
    import requests
    from bs4 import BeautifulSoup
    import os
    
    
    def DownOne(url,name):
        toIdxStr = lambda x : ('000000000'+str(x))[-10:]
    
        urlM3U8 = url
        nameOUTPUT = name.replace(' ','')
        pathOUTPUT = 'out'
        sch = re.search('(.+?/[0-9].{3}/[0-9].{1}/[0-9].{1}/)(.+?).m3u8',urlM3U8)
        headM3U8 = sch.group(1)
        nameM3U8 =sch.group(2)
        tempworkpath = 'aaabbbcccdddeeefffggghhhiii'
    
        resM3U8 = requests.get(urlM3U8)
    
        lstTS = re.findall('.+?ts',resM3U8.text)
    
        if not os.path.exists(tempworkpath):
            os.mkdir(tempworkpath)
        if not os.path.exists(pathOUTPUT):
            os.mkdir(pathOUTPUT)
    
        for i,each in enumerate(lstTS):
            print(i,'of',len(lstTS))
            res = requests.get(headM3U8+each)
            with open(tempworkpath+'/'+nameM3U8+toIdxStr(i)+'.ts', 'wb') as f:
                f.write(res.content)
    
        print('合并')
        print('copy /b '+os.path.abspath(tempworkpath)+'\\*.ts '+os.path.abspath(pathOUTPUT)+'\\'+nameOUTPUT+'.mp4')
        os.system('copy /b '+os.path.abspath(tempworkpath)+'\\*.ts '+os.path.abspath(pathOUTPUT)+'\\'+nameOUTPUT+'.mp4')
        os.system('del '+os.path.abspath(tempworkpath)+'\\*.ts')
    
    if __name__ == '__main__':
        url = input('m3u8 url:')
        name = input('video name:')
        DownOne(url,name)
    

## 爬取全部的视频资源

下方代码中的import DownloaderM3U8 as DOWN，这个库是上面爬一个m3u8的那一坨  
下面的程序这些部分我改了，爬的话要填自己的账号密码等

> logData = { ‘account_name’ : ‘这里是我的账号’  
>  ,‘user_pwd’ : ‘这里是我的密码’  
>  ,‘remember_me’ : ‘N’  
>  }  
>  targetURL = ‘填需要获取资源的页面（云班课进入一个课程，再点资源，然后把链接复制过来）’  
>  user-agent也改了，不太清楚这个是不是必要的

#### html解析思路

这是云班课储存资源的思路

    
    
    <div1 这是包含所有视频的一块>
    	<div2 第一单元的所有视频>  所有的div2的集合对应程序里的d2
    		<div 视频></div>	 一个div2中所有div的集合对应程序里的d3
    		<div 视频></div>
    		<div 视频></div>
    		<div 视频></div>
    		<div 视频></div>
    	</div>
    	<div2 第二单元的所有视频>
    		<div 视频></div>
    		<div 视频></div>
    		<div 视频></div>
    		<div 视频></div>
    		<div 视频></div>
    	</div>
    	<div2 第三单元的所有视频>
    		<div 视频></div>
    		<div 视频></div>
    		<div 视频></div>
    		<div 视频></div>
    		<div 视频></div>
    	</div>
    </div>
    

。

值得一提的是我没有在网页的html中找到m3u8的链接，但是每个视频有一张图片，图片的链接和视频的m3u8链接有许多相同点。  
其中图片的链接中有形如： **'年份/月份/字符串’ **的格式，而m3u8的链接中有形如：** ‘年份/月份/日期/字符串’**
的格式，并且这两段格式中除了图片链接没有日期外，其他完全相同

    
    
    #云班课视频资源获取 开始于2021/03/12
    '''
    目前没有任何try,纯莽
    
    part1 登录 (成功)
    part2 找到所有视频块div(成功)
    part3 获取所有视频对应的m3u8链接(遇到问题)
        问题：
            html中没有找到对应的m3u8链接,但有一张图片的链接,其中有 #年份/月份/字符串# 的部分与我们需要的m3u8链接十分相似
            需要的m3u8链接的该部分格式为 #年份/月份/日期/字符串# 所以尝试枚举已知月份中所有的日期暴力破解
            事实证明这种方法有概率失败,因为有的m3u8链接会与图片连接中的月份有偏差,不保证能完全获取所有的m3u8链接
        可能的解决方案：
            1.尝试枚举一年中的所有日期而不是这个月的(需要时间长,但不失为一种方法,尚未尝试),
              但不排除m3u8链接和img链接的年份也有偏差的可能
            //-----------------------------------------------------------
                2021/03/12:
                    若没有找到正确的m3u8链接,则尝试读取下一个月的所有日期
                    这样可以正确解析出更多的链接,但是仍然不能完全解决问题
            //-----------------------------------------------------------
            
    part4 针对每一个m3u8链接下载其中的所有js链接,并尝试调用命令行指令合并ts文件为mp4(尚未开始,太懒了)
        这一步脱离前三个part,独立进行
    '''
    
    import re
    import requests
    from bs4 import BeautifulSoup
    import DownloaderM3U8 as DOWN
    
    def monthIter(year,month):
        monthMap = { 1:  31 
                    ,2:  28
                    ,3:  31 
                    ,4:  30
                    ,5:  31
                    ,6:  30
                    ,7:  31
                    ,8:  31
                    ,9:  30
                    ,10: 31
                    ,11: 30
                    ,12: 31
        }
        if month == 2:
            if year%400 == 0 or (year%4 == 0 and year%100 != 0):
                monthMap[2] += 1
        return range(1,monthMap[month]+1)
    
    
    def toLinkStr(ss):
        ss = str(ss)
        return '0'+ss if len(ss) == 1 else ss
    
    
    m3u8Head = 'https://video-cdn-2.mosoteach.cn/mssvc/file'
    m3u8Tail = 'm3u8'
    
    logURL = r'https://www.mosoteach.cn/web/index.php?c=passport&m=account_login'
    
    session = requests.session()
    
    logHeader = { 'Referer'      : 'https://www.mosoteach.cn/web/index.php?c=passport'
                 ,'User-Agent'   : '填自己的，好像不填也行，不太清楚'
    }
    logData = {  'account_name' : '这里是我的账号'
                ,'user_pwd'     : '这里是我的密码'
                ,'remember_me'  : 'N'
    } 
    
    logRES = session.post(logURL ,headers = logHeader, data = logData)
    
    targetURL = '填需要获取资源的页面（云班课进入一个课程，再点资源，然后把链接复制过来）'
    
    res = session.get(targetURL)
    soup = BeautifulSoup(res.text,features='lxml')
    d1 = soup.find('div', id = 'res-list-box')
    d2 = d1.find_all('div',class_ = 'hide-div')
    
    namelst = []
    lst = []
    cnt = 0
    jumplst = []
    for i,one in enumerate(d2):
        #print('[第',i,'块]')
        d3 = one.find_all('div',class_ = 'res-row-open-enable res-row preview')
        for j,each in enumerate(d3):
            #print(' 第',j,'个',end = ' ')
            ss = each.find('img', class_ = 'res-icon')['src']
            linkKey = re.search('capture/([0-9].{3})/([0-9].{1})(.+?)jpg?',ss)
            m3u8Middle = m3u8Head + '/' + linkKey.group(1) + '/' + linkKey.group(2) + '/{day}' + linkKey.group(3) + m3u8Tail
            m3u8MiddlePLUS = m3u8Head + '/' + linkKey.group(1) + '/' + '{mth}' + '/{day}' + linkKey.group(3) + m3u8Tail
            cnt += 1 
            print(cnt)
            print(m3u8Middle)
            #'''
            found = False
            zyear  = int(linkKey.group(1))
            zmonth = int(linkKey.group(2))
            for EE in monthIter(zyear,zmonth):
                m3u8Final = m3u8Middle.format(day = toLinkStr(EE))
                rr = requests.get(m3u8Final)
                if re.search('Error',rr.text) == None:
                    lst.append(m3u8Final)
                    namelst.append(each.find('span',class_ = 'res-name')['title'])
                    print(m3u8Final)
                    found = True
                    break
            if found == False:
                zmonth += 1
                if zmonth > 12:
                    zmonth = 1
                    zyear += 1
                    m3u8Middle = m3u8MiddlePLUS
                for EE in monthIter(zyear,zmonth):
                    m3u8Final = m3u8Middle.format(day = toLinkStr(EE), mth = toLinkStr(zmonth))
                    rr = requests.get(m3u8Final)
                    if re.search('Error',rr.text) == None:
                        lst.append(m3u8Final)
                        namelst.append(each.find('span',class_ = 'res-name')['title'])
                        print(m3u8Final)
                        found = True
                        break
            if found == False:
                print('##第',i,'块第',j,'个已被跳过')
                jumplst.append((i,j))
            print('---')
           # '''
                #print(each,end = ' ')
            #print(int(linkKey.group(1)),int(linkKey.group(2)))
    
    print('共跳过个数:', len(jumplst), '\n分别为:')
    for each in jumplst:
        print(each)
    
    print('len(lst) == len(namelst) ?',len(lst),len(namelst))
    
    print('开始抓取')
    for i in range(len(lst)):
        print(namelst[i])
        DOWN.DownOne(lst[i],namelst[i])
    
    
    print('共跳过个数:', len(jumplst), '\n分别为:')
    for each in jumplst:
        print(each)
    

## 不足 与 可以预期的改进（已经不会去改了，所以是可以预期的改进）

#### 不足

这个程序的思路是找到整个页面中所有的m3u8存入一个列表，然后遍历列表依次下载。这样时间非常长，如果中途出错还要从头开始重新来。  
。  
没有try，没有写try的意识，等着以后迎接暴毙吧

#### 可以预期的改进

1.分块获取m3u8，先获取每一个大块，再在每一个大块中获得这一大块中所有视频的m3u8  
2.细化获取项，提供获取某一特定块中所有视频的选项  
3.细化获取项，提供在某一特定块中获取某一特定视频的选项  
4.做出图形界面，以一级二级标签分好大块和每个大块中的视频，可以阵对视频多选，阵对块多选，进行下载

