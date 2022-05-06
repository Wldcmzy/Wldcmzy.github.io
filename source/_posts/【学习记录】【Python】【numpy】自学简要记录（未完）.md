---
title: 【学习记录】【Python】【numpy】自学简要记录（未完）.md
date: 1111-11-11 11:11:11
categories:
  - [基础知识, python, numpy]
tags:
  - OldBlog(Before20220505)
  - python库
---

## 说明 - 2022-05-05
本篇博客为本人原创, 原发布于CSDN, 在搭建个人博客后使用爬虫批量爬取并挂到个人博客, 出于一些技术原因博客未能完全还原到初始版本(而且我懒得修改), 在观看体验上会有一些瑕疵 ,若有需求会发布重制版总结性新博客。发布时间统一定为1111年11月11日。钦此。

自学记录，不专业

## 1.很基础的操作

#### 010.建立一个数组以及查看它的一些属性

看就完了，奥里给


​    
    import numpy as np
    
    lst=[i for i in range(6)]
    
    #---------------------------------------------------
    input('\n\n1.创建两个数组，类型为numpy.ndarray\n')
    array1=np.array(lst)
    array2=np.array([[[1,2,3],[4,5,6]],[[0,0,1],[1,1,2]]],dtype=np.int8)#dtype参数规定数组中数据的数据类型
    
    print(type(array1),type(array2))
    
    #----------------------------------------------------
    input('\n\n2.查看数组的维度和形状\n')
    print('维度：',array1.ndim,array2.ndim)
    print('形状：',array1.shape,array2.shape)


​    
​    
    #-----------------------------------------------------
    input('\n\n3.改变数组形状\n')
    array1_re=array1.reshape(2,3)
    print('改变后：',array1_re)
    print('改变前：',array1)


​    
    #-----------------------------------------------------
    input('\n\n4.查看数组数据类型\n')
    print(array1.dtype,array2.dtype)


​    
    #-----------------------------------------------------
    input('\n\n5.查看数组的大小(貌似就是元素个数)\n')
    print(array1.size,array2.size)


#### 011.最简单的提取信息

提取第几行第几列


​    
    import numpy as np
    
    array=np.array([i for i in range(18)]).reshape(3,6)#reshape的作用是把np.array中创建的一维数组变形
    print('原数组array：\n',array)


​    
    #----------------------------------------------
    input('\n\n1.取原数组的0-1行，1-3列\n')
    array1=array[0:2,1:4]
    print(array1)
    
    #----------------------------------------------
    input('\n\n2.取原素组1-末行的首-4列\n')
    array2=array[1:,:4]
    print(array2)


​    
    #----------------------------------------------
    input('\n\n3.取原数组首-末行的2-末列\n')
    array3=array[:,2:]
    print(array3)


#### 012.最简单的矩阵运算

加减乘 转置


​    
    import numpy as np
    
    array1=np.array([i for i in range(18)]).reshape(3,6)
    array2=np.array([i*3 for i in range(18)]).reshape(3,6)
    print('原数组：\n',array1,'\n\n',array2,sep='')


​    
    #-----------------------------------------------
    input('\n\n两数组相加\n')
    print(array1+array2)


​    
    #-----------------------------------------------
    input('\n\n两数组相减\n')
    print(array1-array2)


​    
    #-----------------------------------------------
    input('\n\n两数组相乘，注意不是矩阵乘法，只是把对应位置的数相乘\n')
    print(array1*array2)


​    
    #-----------------------------------------------
    input('\n\narray1+2\n')
    print(array1+2)
    
    #-----------------------------------------------
    input('\n\narray1转置\n')
    print(array1.T)


矩阵乘法  
numpy.matmul(a,b)  
numpy.dot(a,b)

#### 01a.精度

元素为float16类型的数组和元素为float64类型的数组记录同样的值，扩大10000倍后不一样  
通过 set_printoptions(precision=?) 来设置精度


​    
    import numpy as np
    
    array_float16=np.array([[2.2,4.4],[6.6,8.8]],dtype=np.float16)
    array_float64=np.array([[2.2,4.4],[6.6,8.8]],dtype=np.float64)


​    
    #------------------------------------------------------------------------
    input('\n\n1.精度不同，值会存在误差\n')
    print('原数组：\n',array_float16,'\n\n',array_float64,sep='')
    
    input('\n如果把里面的元素都扩大10000倍\n\n')
    print('扩大后：\n',array_float16*10000,'\n\n',array_float64*10000,sep='')


​    
    #------------------------------------------------------------------------
    input('\n\n2.通过 set_printoptions(precision=?) 来设置精度\n\n')
    np.set_printoptions(precision=6)
    print(array_float64/3)


​    

#### 01b.数组元素默认的数据类型

int:默认int32  
float：默认float64


​    
    import numpy as np
    
    array1=np.array([1,2])
    array2=np.array([1.0,2.0])
    
    print(array1.dtype,array2.dtype)


## 1.5快速创建数组1

zeros 创建指定形状和类型的全 0 数组  
zeros_like 创建另一个数组的形状和类型创建全 0 数组  
ones 创建指定形状和类型的全 1 数组  
ones_like 创建另一个数组的形状和类型创建全 1 数组  
empty 创建指定形状和类型的空数组，不对数据进行初始化  
empt_like 根据另一个数组的形状和类型创建空数组，不对数据尽兴初始化  
full 创建指定形状和类型的数组，全部填上指定的值  
full_like 根据另一个数组的形状和类型创建空数组，全部填上指定的值


​    
    import numpy as np
    
    array=np.arange(48).reshape(4,4,3)
    spt=np.dsplit(array,3)
    
    input('\n\nnp.zeors,参数给数组形状')
    zero1=np.zeros((4,4,1),dtype=np.int8)
    print(zero1)
    
    input('\n\nnp.zeros_like参数给一个数组，创建一个相同形状的数组')
    zero2=np.zeros_like(spt[0],dtype=np.int8)
    print(zero2)


​    
​    
    import numpy as np
    
    array=np.arange(48).reshape(4,4,3)
    spt=np.dsplit(array,3)
    
    input('\n\nnp.ones,参数给数组形状')
    one1=np.ones((4,4,1),dtype=np.int8)
    print(one1)
    
    input('\n\nnp.ones_like参数给一个数组，创建一个相同形状的数组')
    one2=np.ones_like(spt[0],dtype=np.int8)
    print(one2)


​    
​    
    import numpy as np
    
    array=np.arange(48).reshape(4,4,3)
    spt=np.dsplit(array,3)
    
    input('这两种方法不对数据进行初始化')
    
    input('\n\nnp.empty,参数给数组形状')
    empty1=np.empty((4,4,1),dtype=np.int8)
    print(empty1)
    
    input('\n\nnp.empty_like参数给一个数组，创建一个相同形状的数组')
    empty2=np.empty_like(spt[0],dtype=np.int8)
    print(empty2)


​    
​    
    import numpy as np
    
    array=np.arange(48).reshape(4,4,3)
    spt=np.dsplit(array,3)
    
    input('以下两种方法建立某种形状且值全为给定值的数组')
    
    input('\n\nnp.full,参数给数组形状')
    ful=np.full((4,4,1),25,dtype=np.int8)
    print(ful)
    
    input('\n\nnp.full_like参数给一个数组，创建一个相同形状的数组')
    fulll=np.full_like(spt[0],12,dtype=np.int8)
    print(fulll)


## 1.6快速创建数组2

#### 快速创建固定步长等差递变数组

arange（开始，结束，步长） 创建等差递变数组


​    
    import numpy as np
    
    input('numpy.arange(),下面的三个参数分别为开始，结束，步长')
    arr=np.arange(-15,23,0.25)
    print(arr)


#### 快速创建有固定元素个数的等差递变数组

linspace(开始，结束，元素个数)


​    
    import numpy as np
    
    print(np.linspace(-20,72,104))


​    

## 2.比较基础的东西

#### 020.基础索引 、切片


​    
    import numpy as np
    
    array=np.array([i for i in range(125)]).reshape(5,5,5)
    print('原数组:\n',array,sep='')


​    
    #-----------------------------------------------------
    input('\n\n获取第2块，第3行，第4列的元素,以下两种方法等效：\n')
    print(array[2][3][4])
    print(array[2,3,4])
    
    #-----------------------------------------------------
    input('\n\n指定步长的索引：获取第0、3块，第0、2、4行，第3列（基于python列表基础）\n')
    print(array[::3,::2,3])
    
    #-----------------------------------------------------
    input('\n\n中括号表示在该维度上取特定的元素：eg1:获取第0、3、4块,后4行,所有列\n')
    print('eg1:\n',array[[0,3,4],1:],sep='')
    input('\n\n然而当我们箱获得第0,1,2,4块，第0,2,3,4行，只得到了一块四行的数据\n')
    print('eg2:\n',array[[0,1,2,4],[0,2,3,4]],sep='')


#### 021.索引 numpy.ix_()


​    
    import numpy as np
    
    array=np.array([i for i in range(125)]).reshape(5,5,5)


​    
    #-----------------------------------------------------
    input('\n\n解决上面(020)的问题，随心所欲取块行列\n')
    #用np.ix_()包裹[0,1,2,4],[0,2,3,4]
    print(array[np.ix_([0,1,2,4],[0,2,3,4])])
    
    #-----------------------------------------------------
    input('\n\n顺便看一下np.ix_([0,1,2,4],[0,2,3,4])是什么\n')
    print(np.ix_([0,1,2,4],[0,2,3,4]))
    
    #-----------------------------------------------------
    input('\n\n把[0,1,2,4],[0,2,3,4]中的[0,1,2,4]换成[[0,1,2,4]]再转置效果也能达到这个效果\n')
    tmp=np.array([[0,1,2,4]]).T
    print(array[tmp,[0,2,3,4]])


#### 022.组合


​    
    import numpy as np
    
    lst1=[0,1,2,3,4]
    lst2=[4,5,6,7,8]
    lst3=[9,8,7,5,6]


​    
    array1=np.array([lst1,lst2,lst3])
    print(array1)
    
    input( 'next ')
    arr1=np.array(lst1)
    arr2=np.array(lst2)
    arr3=np.array(lst3)
    array2=np.array([arr1,arr2,arr3])
    
    print(array2)


#### 023.bool索引


​    
    import numpy as np
    
    array=np.arange(100)#相当于np.array([i for i in range(100)])


​    
    #-------------------------------------
    input('\n\nbool索引\n')
    array_bool=array%3==0
    print(array_bool)
    
    #-------------------------------------
    input('\n\n保留3的倍数\n')
    array_mod3=array[array_bool]
    print(array_mod3)


## 3.数组存入文件、从文件中导入数组

#### 030.导入txt文件、数组储存为txt文件


​    
    import numpy as np
    
    #--------------------------------------------------
    input('\n\n读取txt文件\n')
    array1=np.genfromtxt('abc.txt',dtype=np.str)
    print(array1)
    
    #delimiter  分隔符
    array2=np.genfromtxt('abc.txt',dtype=np.int16,delimiter=' &QAQ& ')
    print(array2)


​    
    #--------------------------------------------------
    input('\n\n读取txt文件,用numpy.loadtxt貌似和numpy.genfromtxt差不多\n')
    print('但貌似genfromtxt功能强一些，下方改为dtype=np.int16会报错，而genfromtxt不会')
    array3=np.loadtxt('abc.txt',dtype=np.str,delimiter=' &QAQ& ')
    print(array3)


​    
    #--------------------------------------------------
    input('\n\n保存数组为txt\n')
    array1[0,0]=0
    np.savetxt('abc_2.txt',array1,fmt='%s',delimiter=' &QAQ& ')
    #参数依次为 保存为txt文件的名字 要保存的数组的名字 元素储存格式 分隔符
    print('保存完成')


#### 031.二进制储存、导入一个数组


​    
    import numpy as np
    
    array1=np.arange(48).reshape(6,8)
    array2=array1.reshape(8,6)
    
    print('二进制读写')
    
    #--------------------------------------------------
    input('\n\n储存一个数组，array1\n')
    np.save('abc-b.sadfsd',array1)
    print('储存完成')
    print('你会发现储存后文件的后缀名永远是npy(如果不是，会自动补上)')


​    
    #--------------------------------------------------
    input('\n\n读取一个数组，格式npy\n')
    array9=np.load('abc-b.sadfsd.npy')
    print(array9)


​    

#### 032.对于多个数组


​    
    import numpy as np
    
    array1=np.arange(48).reshape(6,8)
    array2=array1.reshape(8,6)
    
    #--------------------------------------------------
    input('\n\n储存多个数组,用numpy.savez()\n')
    np.savez('abc-b.sadfsd',giao=array1,cxkjntm=array2)
    #giao 和 cxknjntm是数组的key值(类似字典)，下面会说
    print('储存完成')
    print('你会发现储存后文件的后缀名永远是npz(如果不是，会自动补上)')


​    
    #---------------------------------------------------
    input('\n\n读取一个或多个数组（读取一个npz文件）\n')
    array_home=np.load('abc-b.sadfsd.npz')
    print(array_home)
    print('发现这样读出了一个地址')
    
    #---------------------------------------------------
    input('\n\n分别输出之前存入的array1 array2\n')
    print(array_home['giao'])
    print(array_home['cxkjntm'])


## 4.数组的合并与拆分

#### 040.列合并1 np.c_[]


​    
    import numpy as np
    
    array1=np.arange(16).reshape(4,4)
    array2=np.array([111,222,333,444])
    
    print('注意array2是个一个四行一列向量，是个列向量：array2.shape:',array2.shape)
    input()
    
    input('列合并,注意np.c_[]中的括号是方括号')
    combie1=np.c_[array1,array2]
    combie2=np.c_[array2,array1]
    
    print(combie1,"\n\n",combie2)


#### 041.列合并2 np.column_stack()

注意参数是元组形式


​    
    import numpy as np
    
    arr1=np.arange(4)
    arr2=np.arange(4,8)
    arr3=np.arange(8,12)
    
    input('列叠加若干个一维列向量\n\n')
    combie1=np.column_stack((arr1,arr2,arr3))
    print(combie1)
    
    input('按列合并数组')
    mid1=np.column_stack((arr1,arr2))
    combie2=np.column_stack((mid1,arr3))
    combie3=np.column_stack((combie2,combie2))
    
    print(combie3)


#### 042.行合并1 np.r_[]


​    
    import numpy as np
    
    array1=np.arange(16).reshape(4,4)
    array2=np.array([111,222,333,444])
    
    print('注意array2是个一个四行一列向量，是个列向量：array2.shape:',array2.shape)
    print('如果使用行合并np.r_[]需要先把array2转化称行向量')
    input()
    
    input('行合并,注意np.r_[]中的括号是方括号')
    combie1=np.r_[array1,array2.reshape(1,4)]
    combie2=np.r_[array2.reshape(1,4),array1]
    
    print(combie1,"\n\n",combie2)


​    

#### 043.行合并2 np.row_stack()

注意参数是元组形式


​    
    import numpy as np
    
    arr1=np.arange(4)
    arr2=np.arange(4,8)
    arr3=np.arange(8,12)
    
    input('行叠加若干个一维列向量\n\n')
    combie1=np.row_stack((arr1,arr2,arr3))
    print(combie1)
    print("注意这里不用先把列向量转化为行向量，因为一维列向量会在函数内转化称二维行")
    
    input('按行合并数组')
    mid1=np.row_stack((arr1,arr2))
    combie2=np.row_stack((mid1,arr3))
    combie3=np.row_stack((combie2,combie2))
    
    print(combie3)


#### 044.拆分split


​    
    import numpy as np
    
    #新建0-63的4x4x4三维数组
    array1=np.arange(64).reshape(4,4,4)
    print(array1)


​    
    input("用 split方法拆分数组，参数见注释\n")
    #用split(ary, indices_or_sections, axis=0)拆分
    
    #axis表示分割轴的方向
    #（解释沿某方向：假设二维数组沿水平方向切割，可以想象有一条水平方向的轴线，沿着找条水平轴线的方向把轴线竖切称若干份，得到若干份数组）
    #关于axis： 自己实验发现axis=0表示以最高维度方向为分割方向，如三维数组沿深度方向分割，二位数组沿垂直方向分割
    #axis=1 ，沿第二高的维度方向分割 以此类推
    
    #当indices_or_sections为整数x，则把数组平分成x份，若不能平分则报错
    #当indices_or_sections为列表，则按照列表中的元素为轴拆分数组
    arr1=np.split(array1,4,2)
    print(arr1,'\n\n')
    arr2=np.split(array1,[1,3],2)
    print(arr2)


#### 045.拆分hsplit,vsplit,dsplit


​    
    import numpy as np
    
    #新建0-63的4x4x4三维数组
    array1=np.arange(64).reshape(4,4,4)
    print(array1)
    
    print('分别使用hsplit,vsplit,dsplit分割\n(horizontally,vertically,depth)\n')
    print('注意：hsplit相当于axis=1,vsplit相当于axis=0，dsplit相当于axis=2')
    print('\n\n\n分别使用h,v,d方法拆分三维数组:')
    
    input('hsplit(ary, indices_or_sections),两参数同split一样\n')
    print(np.hsplit(array1,[1,2,3]))
    
    input('vsplit(ary, indices_or_sections),两参数同split一样\n')
    print(np.vsplit(array1,4))
    
    input('dsplit(ary, indices_or_sections),两参数同split一样\n')
    print(np.dsplit(array1,4))
    
    #貌似拆分数组维度不变


#### 046.根据拆分类比多维数组的合并


​    
    import numpy as np
    
    类比 split      hsplit vsplit dsplit
    有 concatenate  hstack vstack dstack
    
    np.vstack((a,b))  
    效果大概同  
    np.concatenate((a,b),axis=0)#注意(a,b)是元组形式的
    
    v:axis=0
    h:axis=1
    d:axis=2

