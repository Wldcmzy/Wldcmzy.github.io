---
title: 数据预处理  sklearn 或者别的库.md
date: 1111-11-11 11:11:11
categories:
  - OldBlog(Before20220505)
tags:
  - OldBlog(Before20220505)
---

## 说明 - 2022-05-05
本篇博客为本人原创, 原发布于CSDN, 在搭建个人博客后使用爬虫批量爬取并挂到个人博客, 出于一些技术原因博客未能完全还原到初始版本(而且我懒得修改), 在观看体验上会有一些瑕疵 ,若有需求会发布重制版总结性新博客。发布时间统一定为1111年11月11日。钦此。

## 无量纲化

sklearn.preprocessing.MinMaxScaler  
数据归一化  
（数据-最小值）/极差 把数据限制在0-1之间 范围可以改 feature_range


​    
```python
from sklearn.preprocessing import MinMaxScaler

data = [[-10,16],[-5,32],[0,48],[5,64]]

scaler = MinMaxScaler(feature_range = [0,2])
scaler = scaler.fit(data)
#scaler = scaler.partial_fit(data)

#进行数据归一化
result = scaler.transform(data)

#利用归一化结果还原原数据
origin = scaler.inverse_transform(result)

#一步
result2 = scaler.fit_transform(data)
```


​    

sklearn.preprocessing.StandardScaler  
数据标准化  
中心化后 x-均值（均值为零）/标准差  
之后满足均值0方差1 的正态分布


​    
```python
from sklearn.preprocessing import StandardScaler

data = [[-10,16],[-5,32],[0,48],[5,64]]

scaler = StandardScaler()
scaler.fit(data)

#mean_均值, var_方差
print(scaler.mean_, scaler.var_)

result = scaler.transform(data)
print(result.mean(), result.std())
#转化后服从均值为0方差为1的正态分布

origin =  scaler.inverse_transform(result)

#一步
result2 = scaler.fit_transform(data)
```


sklearn.preprocessing.MaxAbsScaler  
所有数据/abs（绝对值最大的值）


​    
```python
from sklearn.preprocessing import MaxAbsScaler

data = [[-10,8],[-5,16],[0,24],[5,32]]

scaler = MaxAbsScaler()
scaler = scaler.fit(data)
result = scaler.transform(data)

origin = scaler.inverse_transform(result)

#一步
result2 = scaler.fit_transform(data)
```


## 缺失值


​    
```python
import pandas as pd
import numpy as np

# 从excel表导入数据
df_t = pd.read_excel(r'D:\EdgeDownloadPlace\3dd40612152202ee8440f82a3d277008\train.xlsx')

print(df_t.info())

# 提取列 所有
print(df_t.loc[:,'uid'])

arr_t_uid = df_t.loc[:,'uid'].values

#转换成二维
arr_t_uid = arr_t_uid.reshape(-1,1)

#找到ca列中为？的行
ca_wenhao =df_t[df_t.loc[:,'ca']=='?']

#替换ca列的？为x
df_t['ca'][df_t['ca']=='?'] = 'x'

print('中位数',df_t.loc[:,'age'].median())
print('平均数',df_t.loc[:,'age'].mean())
print('众数',df_t.loc[:,'age'].mode())
cnt =df_t.loc[:,'age'].value_counts() #把所有的值计数 降序排序
idx = cnt.index #结果降序排序
```


​    

此数据缺失值用？代表 ，直接把？换了。  
如果缺失值为nan 可以直接用pd.drowna() 或pd.fillna()

## 编码 哑变量 ---- 处理分类数据用


​    
```python
import pandas as pd
import numpy as np
from sklearn.preprocessing import LabelEncoder
from sklearn.preprocessing import OrdinalEncoder
from sklearn.preprocessing import OneHotEncoder

df_t = pd.read_excel(r'D:\EdgeDownloadPlace\大数据比赛测试示例\训练集.xlsx')
#删掉 ‘Embarked’列有缺失值的行
df_t.dropna(axis = 0 , subset = ['Embarked'], inplace = True)

#一维
LE = LabelEncoder()
LElabel = LE.fit_transform(df_t['Embarked'].astype(np.str))

#多维
OE= OrdinalEncoder()
OElabel = OE.fit_transform(df_t.loc[:,['Sex','Embarked']])

#独热编码
OHE = OneHotEncoder(categories = 'auto')
OHElabel = OHE.fit_transform(df_t.loc[:,['Sex','Embarked']])
df_ttt = pd.concat([df_t,pd.DataFrame(OHElabel.toarray())],axis = 1)
df_ttt.columns = df_t.columns.tolist()+OHE.get_feature_names().tolist()

print(LE.classes_,OE.categories_,OHE.categories_,sep='\n')

rLE = LE.inverse_transform(LElabel)
rOE = OE.inverse_transform(OElabel)
rOHE = OHE.inverse_transform(OHElabel)
```


## 二值化 分箱 ----连续数据


​    
```python
import pandas as pd
import numpy as np
from sklearn.preprocessing import Binarizer
from sklearn.preprocessing import KBinsDiscretizer

df_t = pd.read_excel(r'D:\EdgeDownloadPlace\大数据比赛测试示例\训练集.xlsx')

df_t.fillna(df_t.loc[:,'Age'].value_counts().index[0],axis = 1,inplace = True)
age_t = df_t['Age'].values.reshape(-1,1)

B = Binarizer(threshold= 24)
result = B.fit_transform(age_t)
```


​    
```python
#参数 encode：ordinal onehot 参考分类
#参数 strategy： uniform 等宽分箱(平分特征)  quantile等位分箱(分箱后每箱数量一致)  kmeans按聚类分箱
KBD_ordinal = KBinsDiscretizer(n_bins = 5, encode = 'ordinal', strategy= 'uniform')
KBDresult_ordinal = KBD_ordinal.fit_transform(age_t)
print(KBDresult_ordinal.ravel())#ravel()的作用是降维

KBD_onehot = KBinsDiscretizer(n_bins = 5, encode = 'onehot', strategy= 'uniform')
KBDresult_onehot = KBD_onehot.fit_transform(age_t)
print(KBDresult_onehot.toarray())
```

