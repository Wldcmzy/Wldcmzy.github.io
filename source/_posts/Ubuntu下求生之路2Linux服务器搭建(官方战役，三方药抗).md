---
title: Ubuntu下求生之路2Linux服务器搭建(官方战役，三方药抗).md
date: 1111-11-11 11:11:11
categories:
  - OldBlog(Before20220505)
tags:
  - OldBlog(Before20220505)
---

## 说明 - 2022-05-05
本篇博客为本人原创, 原发布于CSDN, 在搭建个人博客后使用爬虫批量爬取并挂到个人博客, 出于一些技术原因博客未能完全还原到初始版本(而且我懒得修改), 在观看体验上会有一些瑕疵 ,若有需求会发布重制版总结性新博客。发布时间统一定为1111年11月11日。钦此。

## 声明

本文章仅为萌新探索搭建服务器后写的初级总结，有问题请指出。更深层次的内容请自己去研究Sir大佬或者其他人的资料。

## 准备

首先你需要有服务器或者云服务器，我是2核4gUbuntu系统，据说1核2g带求生之路服务器就没问题。  
我使用的Xshell远程连接服务器，使用Xftp从本地上传文件到服务器(不会的百度搜有教程)

## 资料

官方战役主要参考<https://blog.csdn.net/qq_31555445/article/details/105616307>  
药抗插件资源和后期药抗搭建主要参考都是Sir大佬的<https://github.com/SirPlease/L4D2-Competitive-
Rework>

## 搭建过程

这里就直接完全搬运Sir大佬的教程了

#### 1.在服务器上安装以下环境或工具等

screen 是一个托盘工具， 开服后screen -r 可以看服务器状态

> dpkg --add-architecture i386 # enable multi-arch  
>  apt-get update && apt-get upgrade  
>  apt-get install libc6:i386 # install base 32bit libraries  
>  apt-get install lib32z1  
>  apt-get install screen

#### 2.新建一个账户

其实直接用root用户也可以，新建一个用户可能安全一些。  
这里Sir大佬创建的账户名称是steam,名称可以自定义，但之后要把相应路径中的steam换成你自定义的名称。  
然后login登录。

> adduser steam  
>  adduser steam sudo  
>  login

#### 3.安装steamcmd并部署L4D2环境

下载steamcmd_linux.tar.gz

> wget http://media.steampowered.com/installer/steamcmd_linux.tar.gz

把下载好的steamcmd_linux.tar.gz文件解压

> tar -xvzf steamcmd_linux.tar.gz

进入Steam, 启动成功后你的屏幕上会有**Steam>**标志

> ./steamcmd.sh

匿名登录,有一些游戏不支持匿名登陆下载，但显然L4D2可以

> login anonymous

指定安装路径  
所以你文件的真实路径是/home/steam(这个是你的用户名)/Steam/steamapps/common/l4d2

> force_install_dir ./Steam/steamapps/common/l4d2

安装求生之路

> app_update 222860 validate

如果他提示你成功安装完成了，就可以退出了

> quit

## 配置文件

首先需要先获取Sir大佬在Github上发布的资源，我在上面放过连接了，不会从Github下载东西的自行百度。  
。

#### srcds1文件

在Sir大佬发布的资源中，这个文件的路径为：L4D2-Competitive-Rework-master\Dedicated Server Install
Guide。  
。  
第四行，改成自己的账户名

> SRCDS_USER=“steam”

第10行 ，/home后面第一个小写的steam，改成你的账户名

> DIR=/home/steam/Steam/steamapps/common/l4d2

第37行，改成你自己服务器的IP

> IP=1.3.3.7

第40行，把-ip $IP删掉。Sir大佬的教程是没有这一步的，但是我不删掉的时候没法正常开服。

> PARAMS="-game left4dead2 -ip $IP -port  P O R T \+ s v c l o c k c o r r e c
> t i o n m s e c s 25 − t i m e o u t 10 − t i c k r a t e 100 \+ m a p c 1 m
> 1 h o t e l − m a x p l a y e r s 32 \+ s e r v e r c f g f i l e s e r v e
> r PORT +sv_clockcorrection_msecs 25 -timeout 10 -tickrate 100 +map
> c1m1_hotel -maxplayers 32 +servercfgfile server
> PORT+svc​lockcorrectionm​secs25−timeout10−tickrate100+mapc1m1h​otel−maxplayers32+servercfgfileserverSVNUM.cfg"

然后你就可以把这个scrds1文件放到这里（这是一个绝对路径）  
这个文件应是个启动脚本，至于为什么要放到这里，放到别处行不行，我没有尝试。

> /etc/init.d

#### serve.cfg文件

这个文件主要是配置游戏里的一些参数。  
第6行是你的服务器名。

> hostname “xxxxxxxxxxxx”

第11行意思是把你的服务器设置为私有（也就是只有加入特定steam组的人才可以搜索到，不过服务器进人后好像路人也能匹配进来）

> sv_steamgroup_exclusive “1”

而第九行是服务器所属的steam组号

> sv_steamgroup “xxxxxxxxx”

然后你需要 **把这个文件改名为serve1.cfg** 因为Sir大佬就是这么设置的（他考虑了开很多服，而这篇文章讨论开一个服）

#### 进服后公告栏上显示的内容对应的文件

进入服务器后，服务器提供者对应host.txt，下方信息对应motd.txt  
!match 进入药抗后 ，服务器提供者对应myhost.txt，下方信息对应mymotd.txt  
你可以自由编辑

#### 在服务器复制和替换文件

直接到home/steam（你的账户名）/Steam/steamapps/common/l4d2/left4dead2/把修改后的Sir大佬的资源全复制进去就行了。  
我是现在本地windows修改了文件传上去的。（不会传的搜索Xshell上传文件到服务器）

## 启动服务器

先给srcds1文件加上可执行权限

> sudo chmod +x /etc/init.d/srcds1

以下三种操作分别是重启服务器，启动服务器，停止服务器，分不清哪个是哪个的别开服务器了。

> /etc/init.d/srcds1 restart  
>  /etc/init.d/srcds1 start  
>  /etc/init.d/srcds1 stop

###### 某些问题的解决方案

这里有点记不清楚是哪个文件了,好像是srcds1这个脚本文件。  
开服时遇到了这个错误

> -bash: xxx: /bin/sh^M: bad interpreter: No such file or directory

由于我现在windows编辑文件又上传到服务器，一个文件用vi编辑器打开后，输入

> :set ff

显示的是fileformat=dos，而正常应该是fileformat=unix，所以直接进行修改

> set ff=unix

然后使用上述命令就可以开服了。

#### 其他

###### 关于托盘程序

开服后登录你的新账户是可以看你的服务器状态的

> screen -r

进入之后如果你这样按会关服

> Ctrl + C

如果你想退出来这样按

> Ctrl + A 然后 Ctrl + D

###### 你可以简化你开服的敲码量

比如你可以在某个顺手的目录写个脚本

> vi st.sh

> if [ $1 = “start” ]  
>  then  
>  /etc/init.d/srcds1 “start”  
>  elif [ $1 = “stop” ]  
>  then  
>  /etc/init.d/srcds1 “stop”  
>  elif [ $1 = “restart” ]  
>  then  
>  /etc/init.d/srcds1 “restart”  
>  else  
>  echo “Unknow command. arg should be [start/stop/restart]”  
>  fi

然后就可以直接

> ./st.sh start

## 关于游戏中的一些参数

吹牛逼+讲故事

> 问题解决了，说一下解决过程吧，可能造福以后遇到类似问题的人。  
>  没有大佬解答
> 于是自己开始玄学（我下的插件有很多版本，promod（好像是这个名字，太菜了没印象），zonemod2.1，zonemod1.9，apexmod啥的，我只修改了zonemod2.1。）  
>  .  
>  .  
>
> 具体思路是暴力查找文件，ctrl+f搜索诸如bot、infect、ghost等关键词，最后在\cfg\cfgogl\zonemod\shared_cvars.cfg的文件中找到了confogl_addcvar
> confogl_blockinfectedbots "0"和confogl_addcvar director_allow_infected_bots
> “0”，遂把它们全改成1
> 。这时候进服务器可以看到字幕上出现了bommer的响声，linux服务器上也出现了含有jockey一堆英文，但跑了半张图都没见到特感，白高兴一场。于是怀疑还需要改其他的地方，就继续重新搜索了一遍，反而确定了跟特感有关的大概率就是这个文件。  
>  .  
>  .  
>  回过头来仔细看，blockinfectedbots是“阻碍特感电脑”的意思（英语太渣一开始看的头皮发麻就没有仔细看），所以猜测可能是
> director_allow_infected_bots设为1后导演系统允许特感刷新，但是由于blockinfectedbots把特感的刷新阻塞了，于是把confogl_addcvar
> director_allow_infected_bots 改回0 “0”，进入服务器发现ai特感刷新，被牛+口水一顿乱锤，但是非常开心。  
>  .  
>  .
>
> 总结 ： \cfg\cfgogl\zonemod\shared_cvars.cfg文件 ，把这玩意改成1就行了 confogl_addcvar
> director_allow_infected_bots “0”

人话：  
这里我只改了ZoneMod v2.1插件的一些配置。  
许多参数都写在这个文件里 \cfg\cfgogl\zonemod\shared_cvars.cfg  
我列举我找到且用到的几个：  
。  
限制连推的次数，下面代码表示连推10次进入冷却。而冷却时间是递增的，在15次之后达到最大值。

> confogl_addcvar z_gun_swing_vs_min_penalty 10  
>  confogl_addcvar z_gun_swing_vs_max_penalty 15

阻碍ai特感刷新

> confogl_addcvar confogl_blockinfectedbots “0”

导演系统允许ai特感刷新

> confogl_addcvar director_allow_infected_bots “1”

## 关于服务器管理员

这里我直接从网上找资料抄作业，没有深究。  
。  
先在服务器中找到文件addons\sourcemod\configs\core.cfg打开。找到第55行。这里的_password可以随便取名，可能是服务器的一个变量，假设我们把它改成
_aaaa（记好了后面要考）

> “PassInfoVar” “_password”

然后打开文件addons\sourcemod\configs\admins_simple.ini，拉到最下面加入以下内容，就可以把相应的名称设置为管理员名称

> “名称1” “99:z” “密码1”  
>  “名称2” “99:z” “密码2”

名称1可以设置为你的steam用户名。  
在连接服务器之前，你需要在控制台输入以下代码来修改一些信息，否则你用管理员名称登录，服务器检测到你密码不对会拒绝你进入。
**密码要填写特定管理员名对应的密码**

> setinfo “_aaaa” “密码”

此外，如果有人尝试在游戏中通过setinfo name "xxx"改名刚好改成了管理员名，也会被服务器直接T掉，除非他提前修改了信息

> setinfo “_aaaa” “他改成的管理员名称对应的密码”

只要你使用管理员名称，就有管理员权限。如果你把名称改成了别的，就没有管理员权限。

## 目前尚未解决的问题

  1. ai生还吃药不加血，没解决，有大佬会的话私信或者评论帮一下忙呗。

## 关于游戏更新

游戏更新后服务器是没法正常进入的(你可以尝试强行降低游戏版本，但估计不能正常游戏)，所以要在服务器更新游戏版本。

> ./steamcmd.sh  
>  login anonymous  
>  app_update 222860 validate

如果开了插件服务器，则需要等作者更新发布之后，你下载下来在服务器更新

## 文章历史版本

先堆一点东西占个坑，日后补完

资源和主要参考都是Sir大佬的<https://github.com/SirPlease/L4D2-Competitive-Rework>

zonemod v2.1 打开ai特感总结：

> 问题解决了，说一下解决过程吧，可能造福以后遇到类似问题的人。  
>  没有大佬解答
> 于是自己开始玄学（我下的插件有很多版本，promod（好像是这个名字，太菜了没印象），zonemod2.1，zonemod1.9，apexmod啥的，我只修改了zonemod2.1。）  
>  .  
>  .  
>
> 具体思路是暴力查找文件，ctrl+f搜索诸如bot、infect、ghost等关键词，最后在\cfg\cfgogl\zonemod\shared_cvars.cfg的文件中找到了confogl_addcvar
> confogl_blockinfectedbots "0"和confogl_addcvar director_allow_infected_bots
> “0”，遂把它们全改成1
> 。这时候进服务器可以看到字幕上出现了bommer的响声，linux服务器上也出现了含有jockey一堆英文，但跑了半张图都没见到特感，白高兴一场。于是怀疑还需要改其他的地方，就继续重新搜索了一遍，反而确定了跟特感有关的大概率就是这个文件。  
>  .  
>  .  
>  回过头来仔细看，blockinfectedbots是“阻碍特感电脑”的意思（英语太渣一开始看的头皮发麻就没有仔细看），所以猜测可能是
> director_allow_infected_bots设为1后导演系统允许特感刷新，但是由于blockinfectedbots把特感的刷新阻塞了，于是把confogl_addcvar
> director_allow_infected_bots 改回0 “0”，进入服务器发现ai特感刷新，被牛+口水一顿乱锤，但是非常开心。  
>  .  
>  .
>
> 总结 ： \cfg\cfgogl\zonemod\shared_cvars.cfg文件 ，把这玩意改成1就行了 confogl_addcvar
> director_allow_infected_bots “0”

## 完结撒花

