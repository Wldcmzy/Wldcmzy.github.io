---
title: python graphviz的使用  改日填坑.md
date: 1111-11-11 11:11:11
categories:
  - [基础知识, python, graphviz]
tags:
  - OldBlog(Before20220505)
  -	画图
  - graphviz
---

## 说明 - 2022-05-05
本篇博客为本人原创, 原发布于CSDN, 在搭建个人博客后使用爬虫批量爬取并挂到个人博客, 出于一些技术原因博客未能完全还原到初始版本(而且我懒得修改), 在观看体验上会有一些瑕疵 ,若有需求会发布重制版总结性新博客。发布时间统一定为1111年11月11日。钦此。

抄了半个大致模板 自己研究了研究了研究，现在先会画流程图 改日详细填坑


​    
```python
from graphviz import Digraph
from graphviz import Source
dot = Digraph(
    name='try1',
    comment='',
    filename=None,
    directory=None,
    format="png",
    engine=None,
    encoding='utf8',
    graph_attr={'rankdir':'TB'},
    node_attr={'color':'black','fontcolor':'black','fontname':'FangSong','fontsize':'12','style':'rounded','shape':'box'},
    edge_attr={'color':'red','fontcolor':'#888888','fontsize':'10','fontname':'FangSong'},
    body=None,
    strict=False
)
dot.node('A','A-----',shape = 'rectangle')
dot.node('B','B=====',shape = 'circle')
dot.node('C', 'C-----',shape = 'triangle', color = 'blue', fontcolor = 'green')
dot.node('D','D=====',shape = 'circle')
dot.view()
'try1.gv.png'
dot.edge('A','B','E GAY WOLY GIAO')
dot.edge('A','D','ss')
dot.edge('D','C')
dot.edge('C','A')
dot.edge('C','D')
dot.view()
'try1.gv.png'
```

