---
title: sql学习记录（sql sever）.md
date: 1111-11-11 11:11:11
categories:
  - [基础知识, sql]
tags:
  - OldBlog(Before20220505)
  - 数据库
  - sql
---

## 说明 - 2022-05-05
本篇博客为本人原创, 原发布于CSDN, 在搭建个人博客后使用爬虫批量爬取并挂到个人博客, 出于一些技术原因博客未能完全还原到初始版本(而且我懒得修改), 在观看体验上会有一些瑕疵 ,若有需求会发布重制版总结性新博客。发布时间统一定为1111年11月11日。钦此。

~~其实是补之前没交的sql作业~~

## 1.非常基本的操作

##### 新建数据库


​    
```sql
create database L4C2
```


##### 新建表

PID为主键


​    
```sql
create table L4C2.dbo.Person
(
	PID nchar(15),
	PName nchar(25),
	PRole nchar(10),
	Lucky int,
	Ammo int
	primary key(PID)
)
```


##### 增加列


​    
```sql
alter table L4C2.dbo.Person add Ping int
```


##### 修改列

修改列的数据类型


​    
```sql
alter table L4C2.dbo.Person alter column Lucky varchar(10)
```


##### 删除列


​    
```sql
alter table L4C2.dbo.Person drop column Lucky
```


##### 给表增加行

增加多行，给所有列赋值


​    
```sql
insert into L4C2.dbo.Person values
(1234567,'EVO.T im a','Ellis',700,230),
(1234568,'EVO.Star','Smoker',0,225),
(1234569,'Double.Mayuyu','Coach',80,40),
(1234570,'dec.nv','Nick',0,233),
(1234571,'kain.nv','Charger',0,125);
```


给特定列赋值


​    
```sql
insert into L4C2.dbo.Person (PID,PName,PRole) values (7654321,'NPC.Bilibili.Deadz','spectator')
```


##### 创建索引

按照Ping创建降序索引


​    
```sql
create index idx_Ping on L4C2.dbo.Person(Ping desc)
```


按照Ammo创建升序索引


​    
```sql
create index idx_Ammo on L4C2.dbo.Person(Ping asc)
```


##### 删除索引


​    
```sql
drop index idx_Ping on L4C2.dbo.Person
```


## 2.查询训练

1.查找年级为2001的学生，按学号降序排列


​    
```sql
select STname from University.dbo.Student
where STgrade = 2001
order by STid desc
```


查找年级为2001的学生，按学号升序排列


​    
```sql
select STname from University.dbo.Student
where STgrade = 2001
order by STid asc
```


查询所有选课课程中及格的分数，并将其换算为绩点（60分为1，每增加一分，绩点+0.1）


​    
```sql
select (SCore-60)/10+1 from University.dbo.SC
where SCore >= 60
```


查询课时为48或24的课程的名称


​    
```sql
select CRname from University.dbo.Course x
where x.CRtimes=48 or x.CRtimes=24
```


查询课程名中带有data的课程的编号


​    
```sql
select CRid from University.dbo.Course x
where x.CRname like '%data%'
```


查询所有选课记录中的课程号，不重复显示


​    
```sql
select distinct CRid from University.dbo.SC x
```


老师的平均工资


​    
```sql
select avg(THsalary) from University.dbo.Teacher
```


查询所有学生编号和平均成绩，按照平均成绩降序排序


​    
```sql
select STid,avg(SCore) from University.dbo.SC
group by STid
order by avg(SCore) desc
```


统计所有课程的选课人数和平均成绩


​    
```sql
select CRid,count(CRid) '人数',avg(SCore) '平均成绩'
from University.dbo.SC
group by CRid
```


查询至少选修了三门课的学生号


​    
```sql
select STid from University.dbo.SC
group by STid
having count(*) >= 3
```


查询学号为2001001的学生选修的课程名和成绩


​    
```sql
select CRname,Score 
from University.dbo.SC s, University.dbo.Course c
where s.CRid = c.CRid and s.STid = '2001001'
```


查询所有选了database课的学生id


​    
```sql
select STid
from University.dbo.Course c , University.dbo.SC sc
where c.CRname = 'database' and sc.CRid = c.CRid
```


查询所有选了同一课程的学生对


​    
```sql
select distinct x.STid A,y.STid B
from University.dbo.SC x,University.dbo.SC y
where x.CRid = y.CRid and x.STid<y.STid
```


查询至少有两人选修的课程id


​    
```sql
select CRid
from University.dbo.SC
group by CRid
having count(*) >=2
```


查询跟学号2001001的学生共同上课的学生的编号


​    
```sql
select distinct y.STid
from University.dbo.SC x, University.dbo.SC y
where x.CRid = y.CRid and x.STid = '2001001'and x.STid <> y.STid
```


查询学号为2001001的学生的姓名，以及其所有选课的课程名与成绩


​    
```sql
select distinct s.STname,c.CRname,sc.SCore
from University.dbo.SC sc , University.dbo.Course c , University.dbo.Student s
where s.STid = sc.STid and sc.CRid = c.CRid and s.STid = '2001001'
```


查询所有选了课的学生的基本信息以及课程名和成绩


​    
```sql
select distinct s.STid,s.STname,s.STgrade,c.CRname,sc.SCore
from University.dbo.SC sc , University.dbo.Course c , University.dbo.Student s
where s.STid = sc.STid and sc.CRid = c.CRid
```


查询与学号为2001001同年级的学生的信息


​    
```sql
select *
from University.dbo.Student
where STgrade = (
	select STgrade
	from University.dbo.Student
	where STid = '2001001'
)
```


​    

查询所有选了课的学生信息


​    
```sql
select *
from University.dbo.Student
where STid in (
	select STid
	from University.dbo.SC
)
```


​    

查询没有人选的课程的编号


​    
```sql
select CRid
from University.dbo.Course
where CRid not in (
	select CRid
	from University.dbo.SC
)
```


查询所有选了课程名为cpp的课程的学生姓名


​    
```sql
select STname
from University.dbo.Student
where STid in (
	select STid
	from University.dbo.SC
	where CRid in(
		select CRid
		from University.dbo.Course
		where CRname = 'cpp'
	)
)
```


​    

查询成绩最差的成绩状况


​    
```sql
select * from University.dbo.SC x
where x.SCore <= ALL(
	select SCore from SC where SCore is not NULL
)
```


查询课时跟python或java一样长的课程名


​    
```sql
select CRname from University.dbo.Course
where CRtimes = some(
select CRtimes from University.dbo.Course x
where CRname = 'Java' or CRname = 'Python'
)
```


查询选了1001号课的学生名字


​    
```sql
select STname from University.dbo.Student y
where exists(
	select * from University.dbo.SC x
	where x.CRid = '1001' and y.STid = x.STid
)
```


查询选修了所有课程的学生的姓名

