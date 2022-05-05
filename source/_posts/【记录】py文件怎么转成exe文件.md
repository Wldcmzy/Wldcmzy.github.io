---
title: 【记录】py文件怎么转成exe文件.md
date: 1111-11-11 11:11:11
categories:
  - OldBlog(Before20220505)
tags:
  - OldBlog(Before20220505)
---

## 说明 - 2022-05-05
本篇博客为本人原创, 原发布于CSDN, 在搭建个人博客后使用爬虫批量爬取并挂到个人博客, 出于一些技术原因博客未能完全还原到初始版本(而且我懒得修改), 在观看体验上会有一些瑕疵 ,若有需求会发布重制版总结性新博客。发布时间统一定为1111年11月11日。钦此。

其实转完了之后我也是啥也不会的状态，所有的操作仅仅是照葫芦画瓢

第一步：  
首先你要有 pyinstaller

第二步：  
打开cmd 输入pyinstaller -F --onefile 需要转换的文件  
这样就ok了

附带一些pyinstaller的用法（转自<https://blog.csdn.net/jirryzhang/article/details/78881512>）  
-F, –onefile 产生一个文件用于部署 (参见XXXXX).  
-D, –onedir 产生一个目录用于部署 (默认)  
-K, –tk 在部署时包含 TCL/TK  
-a, –ascii 不包含编码.在支持Unicode的python版本上默认包含所有的编码.  
-d, –debug 产生debug版本的可执行文件  
-w,–windowed,–noconsole 使用Windows子系统执行.当程序启动的时候不会打开命令行(只对Windows有效)  
-c,–nowindowed,–console 使用控制台子系统执行(默认)(只对Windows有效)  
-s,–strip 可执行文件和共享库将run through strip.注意Cygwin的strip往往使普通的win32 Dll无法使用.  
-X, –upx 如果有UPX安装(执行Configure.py时检测),会压缩执行文件(Windows系统中的DLL也会)(参见note)  
-o DIR, –out=DIR 指定spec文件的生成目录,如果没有指定,而且当前目录是PyInstaller的根目录,会自动创建一个用于输出(spec和生成的可执行文件)的目录.如果没有指定,而当前目录不是PyInstaller的根目录,则会输出到当前的目录下.  
-p DIR, –path=DIR 设置导入路径(和使用PYTHONPATH效果相似).可以用路径分割符(Windows使用分号,Linux使用冒号)分割,指定多个目录.也可以使用多个-p参数来设置多个导入路径  
–icon=<FILE.ICO> 将file.ico添加为可执行文件的资源(只对Windows系统有效)  
–icon=<FILE.EXE,N> 将file.exe的第n个图标添加为可执行文件的资源(只对Windows系统有效)  
-v FILE, –version=FILE 将verfile作为可执行文件的版本资源(只对Windows系统有效)  
-n NAME, –name=NAME 可选的项目(产生的spec的)名字.如果省略,第一个脚本的主文件名将作为spec的名字  
————————————————  
版权声明：本文为CSDN博主「jirryzhang」的原创文章  
原文链接：<https://blog.csdn.net/jirryzhang/article/details/78881512>

pyinstaller -F -i icon.ico -w sadf.py

