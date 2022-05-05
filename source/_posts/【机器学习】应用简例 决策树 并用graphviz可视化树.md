---
title: 【机器学习】应用简例 决策树 并用graphviz可视化树.md
date: 1111-11-11 11:11:11
categories:
  - OldBlog(Before20220505)
tags:
  - OldBlog(Before20220505)
---

## 说明 - 2022-05-05
本篇博客为本人原创, 原发布于CSDN, 在搭建个人博客后使用爬虫批量爬取并挂到个人博客, 出于一些技术原因博客未能完全还原到初始版本(而且我懒得修改), 在观看体验上会有一些瑕疵 ,若有需求会发布重制版总结性新博客。发布时间统一定为1111年11月11日。钦此。

导入库

    
    
    import pandas as pd
    import numpy as np
    from sklearn import tree
    from sklearn.model_selection import train_test_split
    

导入数据

    
    
    df_t=pd.read_excel(r'D:\EdgeDownloadPlace\3dd40612152202ee8440f82a3d277008\train.xlsx')
    df_t=df_t.drop(columns='uid')
    df_t
    

![df_t](https://img-blog.csdnimg.cn/20201028221351794.png?x-oss-
process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dsZGNtenk=,size_16,color_FFFFFF,t_70#pic_center)  
用众数替换问号

    
    
    for col in df_t.columns:
        df_t[col][df_t[col] == '?'] = df_t[col].value_counts().index[0] if df_t[col].value_counts().index[0] != '?' else df_t[col].value_counts().index[1]
    df_t
    

![df_t2](https://img-blog.csdnimg.cn/20201028221513308.png?x-oss-
process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dsZGNtenk=,size_16,color_FFFFFF,t_70#pic_center)  
得到float类型矩阵

    
    
    arr_t=df_t.values.astype(np.float32)
    arr_t
    

> array([[61., 0., 2., …, 0., 7., 0.],  
>  [64., 1., 3., …, 0., 7., 1.],  
>  [40., 0., 4., …, 0., 6., 1.],  
>  …,  
>  [65., 0., 3., …, 1., 3., 0.],  
>  [63., 1., 4., …, 0., 7., 0.],  
>  [55., 0., 4., …, 1., 7., 1.]], dtype=float32)  
>  把训练集分成测试集训练集(倒入的全是训练集)
    
    
    Xtrain,Xtest,Ytrain,Ytest = train_test_split(arr_t[:,:-1],arr_t[:,-1],test_size=0.3)
    

实例化决策树，训练模型，查看正确率

    
    
    dtc = tree.DecisionTreeClassifier(criterion="entropy"
                                     ,max_depth=4
                                     ,min_samples_split=10).fit(Xtrain,Ytrain)
    score = dtc.score(Xtest,Ytest)
    score
    

> 0.8140703517587939

画图

    
    
    graph_tree = graphviz.Source(tree.export_graphviz(dtc
                                     ,feature_names = df_t.keys()[:-1]
                                     ,class_names = ['患病','不患病']
                                     ,filled = True
                                     ,rounded = True))
    graph_tree
    

![graph_tree](https://img-blog.csdnimg.cn/20201028221815494.png?x-oss-
process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dsZGNtenk=,size_16,color_FFFFFF,t_70#pic_center)

