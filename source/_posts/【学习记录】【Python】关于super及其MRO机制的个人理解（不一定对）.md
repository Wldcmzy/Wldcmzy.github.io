---
title: 【学习记录】【Python】关于super及其MRO机制的个人理解（不一定对）.md
date: 1111-11-11 11:11:11
categories:
  - [基础知识, python]
tags:
  - OldBlog(Before20220505)
  - 语言机制
---

## 说明 - 2022-05-05
本篇博客为本人原创, 原发布于CSDN, 在搭建个人博客后使用爬虫批量爬取并挂到个人博客, 出于一些技术原因博客未能完全还原到初始版本(而且我懒得修改), 在观看体验上会有一些瑕疵 ,若有需求会发布重制版总结性新博客。发布时间统一定为1111年11月11日。钦此。

以下内容均为个人理解。  
**MRO: Method Resolution Order 方法解析顺序**

如果不用super：


​    
```python
class A:
    def fun(self):
        print('A.fun')

class B(A):
    def fun(self):
        A.fun(self)
        print('B.fun')

class C(A):
    def fun(self):
        A.fun(self)
        print('C.fun')

class D(B , C):
    def fun(self):
        B.fun(self)
        C.fun(self)
        print('D.fun')

D().fun()
```


输出结果： 发现A被实例化了两次

> A.fun  
>  B.fun  
>  A.fun  
>  C.fun  
>  D.fun

如果用super：


​    
```python
class A:
    def fun(self):
        print('A.fun')

class B(A):
    def fun(self):
        super(B , self).fun()
        print('B.fun')

class C(A):
    def fun(self):
        super(C , self).fun()
        print('C.fun')

class D(B , C):
    def fun(self):
        super(D , self).fun()
        print('D.fun')

D().fun()
```


输出结果： 发现A没有被重复实例化，这时由于Python继承中的MRO机制

> A.fun  
>  C.fun  
>  B.fun  
>  D.fun

查资料：

> 在每个类声明之后，Python都会自动为创建一个名为“ **mro**
> ”的内置属性，这个属性就是Python的MRO机制生成的，该属性是一个tuple，定义的是该类的方法解析顺序（继承顺序），当用super调用父类的方法时，会按照__mro__属性中的元素顺序去挨个查找方法。我们可以通过“类名.
> **mro** ”或“类名.mro()”来查看上面代码中D类的__mro__属性值

举例：


​    
```python
class A(object):
    pass

class B(object):
    pass

class C(object):
    pass

class D(A,B):
    pass

class E(B, C):
    pass

class F(D, E):
    pass

print(F.__mro__)
```


输出结果：

> (<class ‘ **main**.F’>, <class ‘ **main**.D’>, <class ‘ **main**.A’>,<class
> ‘ **main**.E’>, <class ‘ **main**.B’>, <class ‘ **main**.C’>,<class
> ‘object’>)

个人猜测MRO机制的顺序为：在深度优先的基础上，避免重复调用，且尽可能晚调用。  
即 F->D->A->Object->B->Object->E->B->Object->C->Object  
变为F->D->A->E->B->C->Objcet

**super(type , obj)**  
obj是type或type子类 的 一个实例，一般是self  
super(type , obj)表示obj【按MRO 第一个排在type后面】的父类  
如：F->D->A->E->B->C->Objcet 中 super(B , F()).function() 调用的使C类的function方法。  
**super()**  
相当于super( **class** , self)

