---
title: 【机器学习】应用简例 使用sklearn交叉验证随机森林.md
date: 1111-11-11 11:11:11
categories:
  - [教练我想学挂边躲牛, 瞎学机器学习]
tags:
  - OldBlog(Before20220505)
  - 随机森林
  - 机器学习
---

## 说明 - 2022-05-05
本篇博客为本人原创, 原发布于CSDN, 在搭建个人博客后使用爬虫批量爬取并挂到个人博客, 出于一些技术原因博客未能完全还原到初始版本(而且我懒得修改), 在观看体验上会有一些瑕疵 ,若有需求会发布重制版总结性新博客。发布时间统一定为1111年11月11日。钦此。

从excel中导入训练集+交叉验证随机森林


​    
    import numpy as np
    import pandas as pd
    from sklearn.ensemble import RandomForestClassifier
    from sklearn.model_selection import cross_val_score
    import matplotlib.pyplot as plt
    
    # 从excel表导入数据
    df_t = pd.read_excel(r'D:\EdgeDownloadPlace\3dd40612152202ee8440f82a3d277008\train.xlsx')
    
    # 删除uid列
    df_t = df_t.drop(columns='uid')
    
    # 把数据中的'?'换成每一列的众数
    for col in df_t.columns:
        idx = df_t[col].value_counts().index
        df_t[col][df_t[col] == '?'] = idx[0] if idx[0] != '?' else idx[1]
    
    # 把pandas.DataFrame数据转化为numpy.darray数据 元素类型为np.float32
    arr_t = df_t.values.astype(np.float32)
    
    # 获得100次交叉验证的总体成绩 这100次验证决策树的个数为1-100
    scores = []
    for i in range(100):
        rfc = RandomForestClassifier(n_estimators = i+10, n_jobs = -1)
        rfc_s = cross_val_score(rfc,arr_t[:,:-1],arr_t[:,-1],cv=10).mean()
        scores.append(rfc_s)
    
    # 画出来
    plt.plot(range(1,101),scores,label = 'RandomForest')
    plt.show()

