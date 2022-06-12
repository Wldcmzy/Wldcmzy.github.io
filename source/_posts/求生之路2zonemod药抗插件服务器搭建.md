---
title: 求生之路2zonemod药抗插件服务器搭建
date: 2022-06-11 22:06:36
categories:
  - [教练我想学挂边躲牛, 游戏, 求生之路2]
tags:
	- 游戏
	- 求生之路2
	- linux
	- 服务器
---

# 前言

以前曾经写过一篇关于开服的博客，但是现在尝试开服的时候发现老博客的一些内容已经不管用了，而且老博客在转移的过程中，格式出了写问题，所以写一个新的。

# 普通L4D2服务器的搭建

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

# ZoneMod药抗插件的安装

## 插件安装

把sir大佬的项目克隆到相应位置就安装好了。

[SirPlease/L4D2-Competitive-Rework: Just refreshing and optimizing the core files a bit, eh? (github.com)](https://github.com/SirPlease/L4D2-Competitive-Rework)

## 基本配置

#### ./cfg/server.cfg

服务器名称，组号，设置为只有组内成员能搜索到。你可以把这个文件改名为`server1.cfg`(后面会说)。

```
hostname "蓝湖畔淅淅沥沥的雨" 
sv_steamgroup "41174273"
sv_steamgroup_exclusive "1" 
```

#### ./Dedicated Server Install Guide/srcds1

修改路径，以及配置IP端口、游戏信息等内容。 

这个文件需要被放在`/etc/init.d/`目录下。

注意PARAMS变量中的`server$SVNUM.cfg"`，这说明你的`./cfg/`路径下要有`server1.cfg`文件。当然你可以定义多种配置，写多个启动脚本。

```shell
SRCDS_USER="steam"

SVNUM=1
IP=0.0.0.0
PORT=27015
NAME=L4D2_Server$SVNUM
PARAMS="-game left4dead2 -ip $IP -port $PORT +sv_clockcorrection_msecs 25 -timeout 10 -tickrate 100 +map c2m1_highway -maxplayers 12 +servercfgfile server$SVNUM.cfg"
```

## 更加方便

编写shell脚本来调用脚本srcds~~(什么牛马套娃)~~

```shell
if [ $2 = "start" ]
then
/etc/init.d/srcds$1 "start"
elif [ $2 = "stop" ]
then
/etc/init.d/srcds$1 "stop"
elif [ $2 = "restart" ]
then
/etc/init.d/srcds$1 "restart"
else
echo "Unknow command. arg should be [start/stop/restart]"
fi
```

示例

```
./startgame.sh 2 restart
相当于
/etc/init.d/srcds2 restart
```

# 服务器配置

## 服务器管理员

在`addons\sourcemod\configs\core.cfg`文件中修改PassInfoVar变量的值，它声明了表示密码的变量的名称， 这个值可以随意更改，也可以不改用sir写的默认值。假设我们改成"_abcd"

```
"PassInfoVar" "_abcd"
```

在`addons\sourcemod\configs\admins_simple.ini`文件中，拉到最下面加入以下内容。

```
"名称1" "99:z" "密码1"
"名称2" "99:z" "密码2"
```

这时候名称1和名称2已经被设为管理员了，如果想使用这两个名称登录，就需要在你的信息中设置对应的密码。

如"名称1"想进入服务器的话，需要先在控制台设置:

```
setinfo "_abcd" "密码1"
```

如"名称2"想进入服务器的话，需要先在控制台设置:

```
setinfo "_abcd" "密码2"
```

否则， 进不去，进去了再改成管理员名称也会被题出来。

## 游戏参数配置 (只写改最新的zonemod插件)

#### 限制连推次数

路径：`./cfg/cfgogl/zonemod/shared_cvars.cfg`

>confogl_addcvar z_gun_swing_vs_min_penalty 10
>confogl_addcvar z_gun_swing_vs_max_penalty 15

## AI特感

路径：`./cfg/cfgogl/zonemod/shared_cvars.cfg`

阻碍ai特感刷新

> confogl_addcvar confogl_blockinfectedbots "0"

导演系统允许ai特感刷新

> confogl_addcvar director_allow_infected_bots "1"

## 安全屋外止疼药数量

路径：`./cfg/cfgogl/zonemod/confogl.cfg`

>confogl_addcvar confogl_pills_limit 4

## 修改止疼药加血量

路径：`./cfg/server.cfg`

>sm_cvar pain_pills_health_value 10 止疼药额外+10(也就是+60) 会影响战役

## 让AI生还者吃药加血

路径：`./addons/sourcemod/plugins/optional/`

> 删除(改名就行)插件botpopstop.smx

## 修改地图信息

路径：`./cfg/stripper/zonemod/maps`

想用原地图把地图配置文件从目录中移除

# 一些错误的解决

## 错误1 

**-bash: /etc/init.d/srcds1: /bin/sh^M: bad interpreter: No such file or directory**

文件编码有问题。

sir的说明:

>If you receive "-bash: /etc/init.d/srcds1: /bin/sh^M: bad interpreter: No such file or directory" error, it means you have dos line ending file You can use dos2unix command on srcds1 file, or use any other method to have this file in unix format

解决方法1:

> vi打开文件， 输入:set ff=unix， 然后:wq退出

解决方法2:

> sed -i -e 's/\r$//' 文件名

# 其他

## 关于游戏更新

>./steamcmd.sh
>login anonymous
>app_update 222860 validate

如果开了插件服务器，则需要等作者更新发布之后，将作者的更新同步到你的服务器

## 关于screen托盘

查看被托盘的进程，只能看到本账户开的，不能看到其他账户开的。

``` 
screen -ls
screen -r
当只有一个进程时, screen -r会直接进入进程, screen -ls 不会
```

打开进程

```
screen -r 进程名称或进程号(pid)
当只有一个进程被托盘时, 输入screen -r直接打开该进程
```

退出进程

```
Ctrl + C 直接杀掉进程
Ctrl + A + D 退出进程页面, 进程被托盘
```

