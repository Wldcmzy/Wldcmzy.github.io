---
title: linux服务器上通过python脚本自动控制mysql数据库储存背单词软件前端传来的数据
date: 2022-05-06 18:35:40
categories:
  - [小设计, 课程设计]
tags:
  - linux
  - socket
  - sql
  - 数据库
  -	服务器
  - 后端
---

## 声明

1.本人还是比较菜的，对服务器安全性等内容可能一窍不通，也没有使用框架，仅考虑完成目的。

2.本篇博客仅记录使用本人的总结，仅论述实现目的的基本流程， 不详细讲解知识，数据库初学者建议先学习sql基本语法(时间成本不高)

##  环境安装

我使用的操作系统是Ubuntu20, 装好之后是自带python3, 所以我们直接无脑安装mysql

```
apt update
apt install mysql-server
```

非root账号输入, 当然要能获取root权限才行

```
sudo apt update
sudo apt install mysql-server
```

## 新建一个账号

这一步没什么用，只是我想新建一个

```
adduser --home /usr/sir sir
```

## mysql登录

mysql数据库创建时会自动生成一串root用户密码,想改密码的可以去查查怎么改。使用root账号可以直接这样登录

```
mysql
```

非root账户用用户名密码登录, 输入下方语句后会提示你输入密码

```
mysql -u username -p
```

## 数据库基本操作

建增删改查等操作略，这里不详细讲解。



创建一个数据库用户, @后跟一个ip, 表示允许哪些ip,或哪些子网段的ip访问, 若写%表示所有, localhost顾名思义是本地

 identified by 后面写密码

```sql
create user 'testdbuser'@'localhost' identified by 'abcd';
```

给用户授权， 当然也可以新建角色给角色授权，然后再把角色权限授权给用户，

```sql
grant all privileges on table 好几个表 to testdbuser@localhost
```



## python自动操作数据库

#### 安装pymysql库

无脑安装

```
pip install mypysql
```

#### pymysql基本操作

```python
import pymysql
```

首先创建**对象**和**游标**， 形如下方代码 

参数database表示你要操作哪个数据库 ，用户名密码要正确，且用户要有访问权限才行

```python
self.__connection = pymysql.connect(host = self.__host, user = self.__user, password = self.__password, database = self.__database) # 实例化对象
        
self.__cursor = self.__connection.cursor(cursor=pymysql.cursors.DictCursor) #游标
```

**增删改查**

注意除了查询操作外，其他操作(也就是会改变数据库内容的操作)需要最后使用**commit()**方法,否则无法成功真正在数据库中完成操作

查

```python
sql = 'select count(*) cnt from User where uid = %s'
self.__cursor.execute(sql, (uid)) # 使用execute执行
cnt = self.__cursor.fetchone()['cnt'] #  使用fetchone() fetchmany() fetchall() 获取返回结果的一条、多条或全部
if cnt == 0: return '无该用户'
```

增

```python
sql = 'insert into User values (%s, %s, %s, %s)'
self.__cursor.execute(sql, (cnt + 1, name, password, False))
self.__connection.commit() #操作设计数据库修改需要执行commit方法
```

其他的都差不多 

根据使用经验，若传参为字符串自带引号，当然可以提前写好之后只execute(sql)

```python
sql = 'insert into User values (%s, %s, %s, %s)' %(乱七八糟)
self.__cursor.execute(sql)
```

#### 部署后端

自己写个类，把想要的功能封装进去，就可以挂在服务器上自动跑脚本了。比如我正在做的东西有以下具体过程:

> 接受前端发来的数据
>
> 数据解密
>
> 验证数据正确性和完整性
>
> 为自己写的数据库操作类创建一个对象，通过外来数据操作数据库
>
> 编辑和加密回馈数据
>
> 回馈前端

## 其他

等学期末东西做完了应该会写一篇完整总结并附代码到github
