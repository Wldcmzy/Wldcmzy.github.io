---
title: linux使用tinyproxy开启代理服务器
date: 2022-07-27 14:58:18
categories:
  - [教练我想学挂边躲牛, 代理服务器]
tags:
  - linux
  - proxy
  - 代理服务器
---

## 安装tinyproxy

Ubuntu系统

```
apt-get install tinyproxy
```

## 修改配置文件

配置文件默认路径为`/etc/tinyproxy/tinyproxy.conf`

> Port 开放端口
>
> Allow 允许的ip地址

## 重启tinyproxy服务

```
sudo service tinyproxy restart
```

## 莫得加密

