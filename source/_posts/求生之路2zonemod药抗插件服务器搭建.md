---
title: 求生之路2zonemod药抗插件服务器搭建
date: 2022-06-11 22:06:36
categories:
  - [游戏, 求生之路2]
tags:
	- 游戏
	- 求生之路2
	- linux
	- 服务器
---

# 前言

以前曾经写过一篇关于开服的博客，但是现在尝试开服的时候发现老博客的一些内容已经不管用了，而且老博客在转移的过程中格式除了写问题，所以写一个新的。

# 正文

主要还是搬sir大佬的文章咯

## 环境和必要组件

```
dpkg --add-architecture i386 # enable multi-arch
apt-get update && apt-get upgrade
apt-get install libc6:i386 # install base 32bit libraries
apt-get install lib32z1
apt-get install screen
```

## 创建一个名为steam的账户

这个用户名是可以随意指定的，但sir大佬的教程里用的steam，~~如果我们也用steam的话就不用手动改一些路径了(什么落伍的老方法，今非昔比了)~~。

如果你不适用steam作为账户名，只需要修改SRCDS_USER变量名就好了(后面会说)。

```
adduser steam
adduser steam sudo
login
```

## 安装steamcmd和求生之路2游戏

这是sir大佬的教学文档里写的。主要做了: 安装steamcmd 匿名登录  更改安装路径 安装求生之路2。

```
(先进入steam账户的根目录)
wget http://media.steampowered.com/installer/steamcmd_linux.tar.gz
tar -xvzf steamcmd_linux.tar.gz
./steamcmd.sh
login anonymous
force_install_dir ./Steam/steamapps/common/l4d2
app_update 222860 validate
quit
```

`(2022/06/11)`但我用的时候会报打不开xxxsteam.so(忘记名字了)库的错误，错误原因未知，我猜测是steamcmd的版本过时了，所以找到了steamcmd的文档[SteamCMD - Valve Developer Community (valvesoftware.com)](https://developer.valvesoftware.com/wiki/SteamCMD#Downloading_SteamCMD)，发现可以直接apt安装(Ubuntu)， 由于运行steamcmd，会自动创建在当前目录创建`.steam`目录，而steamcmd的工作目录在`./.steam/steamcmd`，所以路径想和sir保持一致偷懒的话，路径要修改。

所以流程变成了:

```
(先进入steam账户根目录)
apt install steamcmd
steamcmd
force_install_dir ../../Steam/steamapps/common/l4d2
login anonymous
app_update 222860 validate
quit
```

## 开启无插件的本地服务器

安装完求生之路2之后，就可以开求生服务器了，但现在还是官方版本。

运行`/home/steam/Steam/steamapps/common/l4d2`目录的 `srcds_run`脚本直接开服即可

# 未完待续...