---
title: hexo框架下博客应用夏娜主题并略微魔改
date: 2022-05-06 11:29:57
categories:
  - [教练我想学挂边躲牛, 博客搭建]
tags:
  - 博客搭建
  - git
  - web
  - hexo
---

## 应用夏娜主题

[夏娜主题github连接](https://github.com/ShanaMaid/hexo-theme-shana)

~~我直接进行一个主题README.md文件的搬~~

### 安装

```
 git clone https://github.com/ShanaMaid/hexo-theme-shana themes/shana
```

### 配置

修改hexo根目录下的 `_config.yml`

```  yml
language: zh-CN

theme: shana
```

同时将`themes/shana/_source/`的`tags`和`categories`文件夹拷贝到hexo根目录下的`source`文件夹下

### 更新

```
cd themes/shana

git pull origin master
```



## 瞎搞和魔改过程

**~~由于我不是很懂前端，所以不能随意更改博客，只能走一步看一步~~**

#### 更改网站的favicon

把你想改的图片放到hexo根目录下的source目录中，然后修改shana主图根目录下_config.yml配置文件favicon属性的值为图片名即可，如:

```yml
favicon: /favicon.png
```



#### 修改博客标题、副标题、描述、作者等内容

在hexo根目录修改配置文件_config.yml对应内容即可。

```yml
# Site
title: 蓝湖畔的雨
subtitle: 'All tragedy erased, I see only wonders...'
description: ''
keywords:
author: StarsWhisper
language: zh-CN
timezone: ''
```

#### 增加菜单

一开始应该只有主页、归档、分类、标签四个菜单，可以通过修改shana主题根目录中的_config.yml配置文件来实现添加菜单

```yml
menu:
  Home: /
  Archives: /archives
  Categories: /categories
  Tags: /tags
  About: /about
rss: /atom.xml
```

仿照 **themes/shana/_source/** 中的文件夹，建立about文件夹以及里面的内容，并把它扔到hexo根目录下的`source`文件夹下即可

若不在翻译文件中加入自定义的名称，自定义菜单选项会显示英文，所以我们在 **\themes\shana\languages** 目录下修改zh-CN.yml 添加如下内容

```yml
#你的自定义名称: 中文翻译
About: 关于啊啊啊
```

增加点击菜单后显示的内容，只需要在对应的index.md文件中增加内容即可

#### 增加或修改右键菜单内容

在shana主题配置文件中找到gaymenu_item:，比如我的已经改成了

```yml
gaymenu_item:
- name: 公告
  url : /announcement
- name: 标签
  url : /tags
- name: 分类
  url : /categories
- name: 归档
  url : /archives
- name: 关于
  url : /knightabout
- name: 传送门
  url : /bridges
```

仿照修改就可以

#### 修改右键菜单图标

在 **\themes\shana\source\plugin\galmenu** 目录下修改img.png文件，自己P以下就行。

原图：

![img1](hexo框架下的博客应用夏娜主题并略微魔改/img1.png)

修改后：

![img](hexo框架下的博客应用夏娜主题并略微魔改/img.png)

#### 社交连接和友情连接

修改shana配置文件， 比如:

```yml
# 社交链接
social:
- url: https://github.com/Wldcmzy
  name: Github
- url: https://blog.csdn.net/wldcmzy
  name: CSDN
- url: https://space.bilibili.com/83743701
  name: bilibili(无技术和学习内容)

# 友情链接
# name : url
link_title: 友情链接
links:
  夏娜主题作者的博客: https://shanamaid.github.io/
```

#### 修改头像

在shana配置文件中更改avatar属性的值为头像图片的url即可。

若想使用本地图片，则在 **shana/source/** 目录下创建images文件夹，并把头像图片扔到里面，avatar的值这样写:

```yml
avatar: /images/avatar.png(这个是你的头像图片名)
```

#### 修改背景图

在 **\shana\source\plugin\bganimation** 目录下，直接把你想用的背景图片放进去。

然后修改文件bg.css

下方代码中对应的图片名部分改为自己的背景图片名，也可以使用图片对应的url

```css
.cb-slideshow li:nth-child(1) span { background-image: url('qianbei.jpg');}
.cb-slideshow li:nth-child(2) span { background-image: url('sister.jpg');}
.cb-slideshow li:nth-child(3) span { background-image: url('brother.jpg');}
.cb-slideshow li:nth-child(4) span { background-image: url('shadow.jpg');}
.cb-slideshow li:nth-child(5) span { background-image: url('knight.jpg');}
.cb-slideshow li:nth-child(6) span { background-image: url('kuiruo.jpg');}
```

#### 修改鼠标指针

在 **\shana\source\css\images/** 目录下修改icon.png文件，可以自己找图或者p一个或者画一个。喜欢小骑士的可以用我这个:

![icon](hexo框架下的博客应用夏娜主题并略微魔改/icon.png)

#### 修改代码高亮、代码背景等

对应文件 **\shana\source\css\ _partial\highlight.styl** 

修改代码背景: 修改下方代码中 **background** 的值

```stylus
$code-block
  border-left: 5px solid #0000FF
  background: #2244AA
  margin: 0 article-padding * -1
  padding: 15px article-padding
  overflow: auto
  color: highlight-foreground
  line-height: font-size * line-height
```

#### 修改文章版透明度

修改文件 **\themes\shana\source\css\ _variables.styl **中color-background-rgba属性的第四个值

#### 更换换页按钮的文本

修改文件 **themes\shana\layout\_partial\archive.ejs** 中最下方对应内容

```ejs
<% if (page.total > 1){ %>
  <nav id="page-nav">
    <%- paginator({
      prev_text: "<<< 上一页",
      next_text: "下一页 >>>"
    }) %>
  </nav>
<% } %>
```

