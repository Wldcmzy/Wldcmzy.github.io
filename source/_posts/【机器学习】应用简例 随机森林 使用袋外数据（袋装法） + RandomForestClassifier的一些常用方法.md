---
title: 【机器学习】应用简例 随机森林 使用袋外数据（袋装法） + RandomForestClassifier的一些常用方法.md
date: 1111-11-11 11:11:11
categories:
  - OldBlog(Before20220505)
tags:
  - OldBlog(Before20220505)
---

## 说明 - 2022-05-05
本篇博客为本人原创, 原发布于CSDN, 在搭建个人博客后使用爬虫批量爬取并挂到个人博客, 出于一些技术原因博客未能完全还原到初始版本(而且我懒得修改), 在观看体验上会有一些瑕疵 ,若有需求会发布重制版总结性新博客。发布时间统一定为1111年11月11日。钦此。


    import numpy as np
    import pandas as pd
    from sklearn.ensemble import RandomForestClassifier
    
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
    
    
    
    rfc = RandomForestClassifier(bootstrap=True #boostrap默认为True 即使用装袋法
                                 ,oob_score=True #oob_score默认为False 为True即使用袋外数据做测试集，训练模型时不需要划分训练集和测试集#
                                 ,max_depth=4
                                 )
    rfc = rfc.fit(arr_t[:,:-1],arr_t[:,-1])
    
    print(rfc.oob_score_) #打印成绩
    
    
    print(rfc.estimators_) #打印所有的树
    
    print(rfc.feature_importances_) #打印特征的重要性
    
    Xtrain,Xtest,Ytrain,Ytest = train_test_split(arr_t[:,:-1],arr_t[:,-1],test_size=0.3)
    
    rfc = RandomForestClassifier(max_depth=4).fit(Xtrain,Ytrain)
    
    print(rfc.apply(Xtest)) #返回测试集中 每一个样本 在每一颗树中的叶子节点的索引
    
    print(rfc.predict(Xtest)) #返回模型预测的测试集的结果
    
    print(rfc.predict_proba(Xtest)) #返回每一个样本被分到每一个标签的概率
    

