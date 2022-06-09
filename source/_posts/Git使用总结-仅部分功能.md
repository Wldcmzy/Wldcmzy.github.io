---
title: Git使用总结(仅部分功能)
date: 2022-06-09 22:01:28
categories:
  - [基础知识, git]
tags:
	- git
---

## 部分全局配置

修改全局配置

```
git config --global 配置名称 配置新值
```

部分全局配置

用户名

```
git config user.name
```

邮箱

```
git config user.email
```

HTTP和HTTPS代理

```
git config http.proxy
git config https.proxy
```



## 新建一个库时Github给出的初始化代码

```git
echo "# New Repo" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/Wldcmzy/234.git
git push -u origin main
```

#### 解析

初始化

```
git init
```

建立READM.md 文件

```
echo "# New Repo" >> README.md
```

添加README.md 至工作缓存

```
git add README.md
```

将缓存提交至本地库

```
git commit -m "first commit"
```

更改当前分支名字为main

```
git branch -M main
```

设置远程连接的仓库

```
git remote add origin https://github.com/Wldcmzy/234.git
```

将本地仓库提交至远程仓库（将本地origin提交至远程main分支, 写-u设默认，以后只需要git push）

```
git push -u origin main
```

#### 可以不修改分支名字

```git
git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/Wldcmzy/234.git
git push -u origin master
```

## 从远程获取最新数据

```
git pull
```

```
git pull origin master
```

## 其他分支的切换和提交

#### 切换分支

```
git checkout 分支名称
```

#### 新建分支

```
git checkout -b 分支名称
```

#### 新建空的分支

使用`git checkout -b`命令创建的分支是有父节点。

使用`git checkout --orphan`命令，创建孤立分支

```
git checkout --orphan
```

使用`git rm -rf .`来清除拷贝的内容

```
git rm -rf .
```

现在获得了一个空的文件夹，但是现在分支没完全有，使用`git branch -a`也是看不到新分支的。

再随便加一点东西（比如README.md），提交上去。

```
git add .
git commit -m 'new branch'
```

然后分支就有了，可以把它提交到远程仓库。

```
git push origin 新分支
```

