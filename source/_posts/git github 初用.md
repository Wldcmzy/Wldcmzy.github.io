---
title: git github 初用.md
date: 1111-11-11 11:11:11
categories:
  - OldBlog(Before20220505)
tags:
  - OldBlog(Before20220505)
---

## 说明 - 2022-05-05
本篇博客为本人原创, 原发布于CSDN, 在搭建个人博客后使用爬虫批量爬取并挂到个人博客, 出于一些技术原因博客未能完全还原到初始版本(而且我懒得修改), 在观看体验上会有一些瑕疵 ,若有需求会发布重制版总结性新博客。发布时间统一定为1111年11月11日。钦此。

指定仓库的用户名和邮箱，–global表示这台机器的所有仓库都用这个用户名或邮箱

> wldcmzy@DESKTOP-UL0SE7R MINGW64 /D/test (master)  
>  $ git config --global user.name “wldcmzy”  
>  wldcmzy@DESKTOP-UL0SE7R MINGW64 /D/test (master)  
>  $ git config --global user.email “wldcmzy@163.com”

将一个目录变为git可以管理的仓库

> cd 一个目录  
>  git init

> git add：将文件提交到暂存区  
>  git commit -m：将暂存区文件提交到仓库（单引号内为注释）

> git status：检查当前文件状态 git diff：查看文件修改的内容

> git log：获得历史修改记录  
>  git log --pretty=oneline：使记录只显示主要的内容，一行显示

> git reflog：获取历史版本号  
>  git reset --hard 版本号：回退到该版本号对应的版本  
>  git reset --hard HEAD^：回退到上1个版本  
>  git reset --hard HEAD^^：回退到上2个版本  
>  git reset --hard HEAD~50：回退到上50个版本

好像是创造一个readme文件，否则以后push会出错

> git pull --rebase origin master

> git remote add origin https://github.com/Wldcmzy/test.git 关联仓库

> git push origin master 推送到仓库

参考资料  
<https://www.cnblogs.com/imyalost/p/8777684.html>  
<https://www.cnblogs.com/imyalost/p/8762522.html>

