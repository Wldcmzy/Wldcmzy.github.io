---
title: windows环境零基础使用hexo框架+Github搭建个人博客
date: 2022-05-05 21:05:27
categories:
  - [教练我想学挂边多牛, 博客搭建]
tags:
  - 博客搭建
  - git
  - web
  - hexo
---

## 介绍

**Hexo**是一个基于**nodejs**的<u>**静态**</u> **(划重点)** 博客网站生成器，可以让用户通过简单的操作快速生成一系列静态文件。

此外利用它搭建博客，还需要版本控制系统**Git**

## 前置

#### Nodejs 

在[Nodejs官网](https://nodejs.org/zh-cn/)下载一个长期维护的稳定版本(LTS版本)后，无脑下一步安装即可

安装完成后，打开**命令行** *(按组合键win+r，在弹出窗口中输入cmd后按回车)*，分别输入以下语句，若成功显示版本号，则说明安装成功。其中**npm**是Nodejs包管理和分发工具。

```
node -v
npm -v
```



#### Git

从[Git官网](https://git-scm.com/)下载**Git**， 可以使用与上面相似的方法查看Git是否安装成功

```
git --version
```



## Hexo的安装

我们先找好或新建一个文件夹(最好是空文件夹)，当作储存博客信息的地方，并在这个文件夹中通过右键菜单打开**Git Bash**(与命令行类似)

在**Git Bush**中输入下方语句安装**hexo**

```
npm install hexo-cli -g
```

成功安装之后，它会反馈给你一个版本以及安装时间(至少我安装的时候是这样)，你也可以通过命令来查看版本明细

```
hexo -v
```

## 生成静态文件并将网页服务在本地运行

按顺序执行下方代码:

初始化操作，这一步操作后，生成网页的基础条件就达成了

```
hexo init
```

生成网页，由于现在还没有对相应文件做任何改动，所以生成默认的landscape主题网页

```
hexo g
```

在本地运行网页服务

```
hexo s
```

默认在你机器的4000端口开启网页服务(因为刚才的指令没有加别的参数)，可以在浏览器中输入localhost:4000查看

但是一旦你在**Git Bush**中关闭了服务，网页服务就会终止，若想让网页长久存在，需要把它发布到互联网(挂到github的服务器)上。

## 把网页挂到互联网上

#### 建立github仓库

若没有github账号需要先注册。

新建一个名称为: **你的用户名.github.io**的仓库，其他配置可以都不管。

建立仓库的方法是：

>1.点击右上角你的头像，在下拉框中点击**Your repositories**
>
>2.你能看到一个绿色的New，点击它创建一个新的仓库
>
>3.仓库名必须要是"你的用户名.github.io" 其他的不用管，直接点Create repositories创建仓库

#### 生成SSH key 

先配置一些变量，这算是你的身份标识，在对应的地方填写你的用户名和注册邮箱

```
git config --global user.name "用户名"
git config --global user.email "注册邮箱"
```

输入这两条语句可以查看这两个变量是否修改成功

```
git config user.name
git config user.email
```

然后生成ssh key 一直按回车就行

```
ssh-keygen -t rsa -C "注册邮箱"
```

然后在你电脑的 **C:\Users\你的windows用户名\.ssh ** 目录下会生成key的信息， 其中Users目录的名字在你的电脑上可能汉化成了"用户"，若如此，你应该寻找**C:\用户\你的windows用户名\.ssh **目录

#### 把SSH key添加到Github

我们需要在Github中新建SSH key

> 1.点击右上角你的头像，在下拉菜单中点击**Settings**
>
> 2.在左边找到**SSH and GPG keys**选项卡，点击
>
> 3.点击哪个绿油油的**New SSH key**
>
> 4.你需要确认一件事，在.ssh目录中，后缀名为id_rsa.pub文件是公钥，没有后缀名的id_rsa文件是私钥，公钥是可以任何人知道的，而私钥你必须保密(感兴趣的去查询非对称密码体制，RAS算法)，所以你在下一步一定要保证你上传的是公钥
>
> 5.在Title中随便为SSH key取一个名字，用记事本打开.ssh目录中的公钥**id_rsa.pub**原封不动地复制粘贴到Key框中，开头结尾不要多加空格，你添加的东西和公钥内容完全相同。然后点击Add SHH key

#### 将网页部署到Github

打开你存放博客信息的文件夹，在刚才用hexo init命令初始化出来的文件中找到配置文件 **_config.yml** ，用记事本打开它，拉到最下面，修改以下信息，没有的就自己加上。(实际上repo对应的就是你仓库的连接)(我猜会不会跟"种族主义"有关，github现在建议你将master分支改成main，如果你改了的话，branch应该对应main而不是master，如果你没改就当我放P)

```
deploy:
  type: git
  repo: https://github.com/你的Github用户名/你的Github用户名.github.io.git
  branch: master
```

在**Git Bush**输入以下命令，安装deploy-git

```
npm install hexo-deployer-git --save
```

然后输入命令

```
hexo clean (清空hexo缓存(有时你更新了网页内容却发现没有更新，建议清空一下缓存))
hexo generate (根据你当前的配置生成网页文件)
hexo deploy (把网页部署到github服务器上)
```

这三句可以简写为

```
hexo clean
hexo g
hexo d
```

你还记得hexo s是干嘛的吗?

**在浏览器中输入 你的用户名.github.io即可查看博客**

## 注意

1.国内网络访问Github可能非常不稳定，有时候传不上去，可以尝试多试几次或者搭个梯子。很长时间传不上去也无所谓，在本地测试好明天再传。

2.如果你改了东西在浏览器上体现不出来，不妨试一下ctrl+f5刷新浏览器缓存。

3.文件传到Github后，在服务器上部署需要花一段时间，可以在你仓库中点击Actions菜单查看，部署完成后，再访问才会显示修改后的结果。

## 完结撒花

关于如何发布博客、让博客应用其他的主题，以及想看我这个啥也不会的垃圾怎么摸索魔改博客的，欢迎来读我的其他文章

