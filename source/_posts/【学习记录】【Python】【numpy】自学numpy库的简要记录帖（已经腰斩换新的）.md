---
title: 【学习记录】【Python】【numpy】自学numpy库的简要记录帖（已经腰斩换新的）.md
date: 1111-11-11 11:11:11
categories:
  - OldBlog(Before20220505)
tags:
  - OldBlog(Before20220505)
---

## 说明 - 2022-05-05
本篇博客为本人原创, 原发布于CSDN, 在搭建个人博客后使用爬虫批量爬取并挂到个人博客, 出于一些技术原因博客未能完全还原到初始版本(而且我懒得修改), 在观看体验上会有一些瑕疵 ,若有需求会发布重制版总结性新博客。发布时间统一定为1111年11月11日。钦此。

## 1.极其基础的操作

    
    
    import numpy as np
    
    #创建一个数组 数组的类型为numpy.ndarray,其中数据的类型为numpy.int32s
    array1=np.array([[[1,2,3],[4,5,6],[7,8,9]],[[10,11,12],[13,14,15],[16,17,18]]],dtype=np.int32)
    print(array1,'\ntype of array1 is:',type(array1))
    
    #查看数组维数
    print('dimensions of array1:',array1.ndim)
    
    #查看数组形状
    print('shape of array1:',array1.shape)
    
    #改变数组形状
    array2=array1.reshape(6,3)
    print(array2)
    
    #提取数据(array2是二维数组)
    print(array2[1:4,0:2])          #2、3、4行前两列
    print(array2[1:4][0:2])         #两次切片  [1:4]切完后再[0:2]
    print(array2[:,2])              #第三列
    print(array2[:4])               #前四行
    print(array2[:4,:])             #前四行
    
    #数组的大小
    print('size of array1 and array2:',array1.size,array2.size)
    
    #矩阵计算
    print(array1+array2.reshape(2,3,3))     #矩阵相加
    print(array1*array2.reshape(2,3,3))     #矩阵相乘
    print(array1+2)                         #矩阵元素+2
    

## 2.索引

    
    
    import numpy as np
    
    array1=np.array([[[1,2,3],[4,5,6],[7,8,9]],[[10,11,12],[13,14,15],[16,17,18]]],dtype=np.int32)
    
    
    #取元素   第一维的一、二项，第二维的第一项，第三维的一、三项
    array2=array1[np.ix_([0,1],[0],[0,2])]
    print(array2)
    
    #取 array1[0,0,0],以及 array1[1,2,1](最终取出两个数，1和17:[1,17])
    array3=array1[[0,1],[0,2],[0,1]]
    print(array3)
    
    #顺序变的
    array4=array1.reshape(6,3)[[0,5,3,1]]
    print(array4)
    
    #bool索引
    array3=np.array(range(100))     #建立一个0-99的一维数组
    print(array3)
    
    array3Bool=array3%3==0          #意会
    print(array3Bool)
    
    array3Final=array3[array3Bool]          #得到array3中是3的倍数的项
    print(array3Final)
    array3Final_another=array3[array3%3==0] #得到array3中是3的倍数的项
    print(array3Final_another)
    
    

## 3.简单文件储存

    
    
    import numpy as np
    
    #txt
    x=np.genfromtxt('abc.txt',delimiter='&&',dtype=np.str,usecols=1)        #载入文件
    print(x)
    #delimiter 分割值
    #dtype 数据类型
    #usecols 不太清楚
    
    y=np.loadtxt('abc.txt',delimiter='&&',dtype=np.str,usecols=1)           #载入文件
    print(y)
    
    np.savetxt('bca.txt',y,fmt='%s')
    
    
    print('分割线-----------------------')
    #二进制
    
    np.save('b_bca.npyy',y)                     #使用此方法保存的后缀名文件总为npy:  b_bca.npyy.npy
    
    np.savez('b_z_bca.npz',the_x=x,the_y=y)     #此方法可以保存多个数组，后缀名为npz  the_x等形参名对应导入后的操作的键名
    
    tmp_a=np.load('b_bca.npy')
    print(tmp_a)
    
    tmp_b=np.load('b_z_bca.npz')
    print(tmp_b)
    print(tmp_b['the_x'])                   #类似字典，key值为先前保存时的形参名，如the_x
    print(tmp_b['the_y'])
    

## 4.简单练习 && 矩阵的合并与转置

    
    
    import numpy as np
    
    np.set_printoptions(precision=1,threshold=3000,suppress=True)
    #precision控制小数点位数
    #threshold控制输出值的个数，其他的以...代替(查的资料上这么说的，实际没看出效果)
    #suppress=True 取消使用科学计数法
    
    
    array1=np.genfromtxt(r'D:\0我的下载\上证指数\上证指数副本.txt',usecols=(0,4,6),)
    print(array1)
    
    daySpan=(array1[:,0]>=20180901)&(array1[:,0]<20190101)  #筛选出其中2018年9月1日至2019年1月1日的信息
    array2=array1[daySpan]                                  #筛选出其中2018年9月1日至2019年1月1日的信息
    print(array2)
    
    #纵向合并
    #把array1第三列大于0的设为1，不大于0的设为-1，添加至数组最右边
    tmp=array1[:,2].copy()
    tmp[tmp>0]=1
    tmp[tmp<=0]=-1
    array_new=np.c_[array1,tmp]#等同于array_new=np.column_stack((array1,tmp))
    print(array_new)
    
    #横向合并
    arr1=np.array([[1,2,3],[4,5,6],[7,8,9]])
    arr2=np.array([['x','x','x']])
    
    arr_new=np.r_[arr1,arr2]#等同于arr_new=np.row_stack((arr1,arr2))
    print(arr_new)
    
    #一维数组默认时列向量，要转化为行向量才能横向合并
    #eg:[1,2,3]这是一个列向量 (3,)
    #eg:[[1,2,3]]这时一个行向量(1,3)
    
    
    
    #矩阵的转置
    print(arr_new.T)
    

## 5.矩阵的切割

    
    
    import numpy as np
    
    #新建0-15的数组
    array1=np.arange(16).reshape(4,4)
    print(array1)
    
    
    #用hsplit(ary,indices_or_sections)拆分   (相当于split的axis参数=1)
    #纵向拆分
    #第二个参数：如果时整数x，则把数组平均分成x份，不能平均分会报错
    #第二个参数，如果时列表，会以列表中的值为轴拆分
    h_arr1=np.hsplit(array1,4)
    print(h_arr1)
    h_arr2=np.hsplit(array1,[1,3])
    print(h_arr2)
    
    
    #用split(ary, indices_or_sections, axis=0)拆分
    #用法基本同hsplit一样，只不过hsplit只能纵向拆分，split可以定义拆分维度
    #axis=0第一维，即横向拆分，axis=1第二维，即纵向拆分
    arr1=np.split(array1,4,0)
    print(arr1)
    arr2=np.split(array1,[1,3],0)
    print(arr2)
    
    

## 6.图像通道的分离与合成

**分离：**  
img1B,img1G,img1R=np.dsplit(img1,3)  
img1B,img1G,img1R=cv2.split(img1)都可以  
**合成：**  
cv2.merge([b,g,r])  
cv2.merge([zeros, zeros, img1R])

**注意倒数第五句的注释**

    
    
    import cv2
    import numpy as np
    
    def showone(img):
        cv2.imshow('show image',img)
        cv2.waitKey(0)
        cv2.destroyAllWindows()
    def showone_color(img,color):
        zeros=np.zeros(img.shape[:2],dtype='uint8') #记得调整格式
        if color == 'B':
            cv2.imshow('blue',cv2.merge([img1B,zeros,zeros]))
        elif color == 'G':
            cv2.imshow('green', cv2.merge([zeros, img1G, zeros]))
        elif color == 'R':
            cv2.imshow('red', cv2.merge([zeros, zeros, img1R]))
        cv2.waitKey(0)
        cv2.destroyAllWindows()
    def showone_merge(b,g,r):
        cv2.imshow('merge',cv2.merge([b,g,r]))
        cv2.waitKey(0)
        cv2.destroyAllWindows()
    
    
    img1=cv2.imread('theimg.png',1)
    
    img1B,img1G,img1R=np.dsplit(img1,3)
    #img1B,img1G,img1R=cv2.split(img1)
    
    print(img1B.dtype)          #矩阵元素的格式为uint8
    
    
    
    showone(img1B)      #这里图像为灰色，原因是当调用cv2.imshow的时候G、R两个通道被默认设为了与B通道一样，三通道一样时显示灰度图
    
    
    showone_color(img1B,'B')
    showone_color(img1G,'G')
    showone_color(img1R,'R')
    
    showone_merge(img1B,img1G,img1R)        #合成后是原图
    
    

