---
title: hexo框架下博客的创建、编辑和发布
date: 2022-05-06 10:48:25
categories:
  - [教练我想学挂边多牛, 博客搭建]
tags:
  - 博客搭建
  - git
  - web
  - hexo
---

## 创建一篇博客

```
hexo new "博客名称"
```

之后会提示你在特定的目录下创建了对应博客的md文件，以markdown格式编辑文件即可。

markdown格式基本语法不在本文记录范围之内。

## 插入本地图片

在hexo根目录的_config.yml文件中更改

```
post_asset_folder: true
```

之后你使用 hexo new 命令创建新博客时，也会在同目录下创建同名文件夹

然后安装插件hexo-image-link

```
npm install hexo-image-link --save
```

在创建好的同名文件夹中放置图片，并在md文件中以相对路径引用即可，生成博客后可以看到图片

## 使用数学公式

在md文件中使用数学公式在网页中是体现不出来的，需要更换插件以及更改配置文件

```
npm un hexo-renderer-marked --save
npm i hexo-renderer-markdown-it-plus --save
```

在hexo根目录下的_config.yml文件中添加内容

```
markdown_it_plus:
  highlight: true
  html: true
  xhtmlOut: true
  breaks: true
  langPrefix:
  linkify: true
  typographer:
  quotes: “”‘’
  pre_class: highlight
```

## 标签与分类

Front-matter 是文件最上方 `---` 之间的区域，可以通过修改Front-matter来添加标签与分类，比如本文为：

```
---
title: 使用hexo框架发布博客
date: 2022-05-06 10:48:25
categories:
  - [教练我想学挂边多牛, 博客搭建]
tags:
  - 博客搭建
  - git
  - web
  - hexo
---
```

若是单级分类直接写分类名就行，多级分类需要像上面一样使用中括号
