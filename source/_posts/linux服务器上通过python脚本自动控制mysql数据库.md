---
title: python语言实现服务器对mysql数据库的自动处理
date: 2022-06-06 20:55:40
categories:
  - [后端, 数据库]
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

建增删改查等操作示例。

```mysql
select ifUsed from invitation where invitationCode = 'xxx';
update invitation set ifUsed = 'xxx' where invitationCode = 'xxx';
delete from invitation where username = 'xxx';
insert into userInfo values ('xxx', 'xxx', 'xxx', 0, 0, 0);
```



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

##### 第一步

首先创建**对象**和**游标**， 形如下方代码 

参数database表示你要操作哪个数据库 ，用户名密码要正确，且用户要有访问权限才行

```python
self.__connection = pymysql.connect(host = self.__host, user = self.__user, password = self.__password, database = self.__database) # 实例化对象
        
self.__cursor = self.__connection.cursor(cursor=pymysql.cursors.DictCursor) #游标
```

##### 增删改查

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

##### 长时间未操作连接断开

Connection对象的ping 方法可以检查是否连接还在，将参数reconnect设为True表示若连接断开自动重连

```python
self.__connection.ping(reconnect=True)
```

