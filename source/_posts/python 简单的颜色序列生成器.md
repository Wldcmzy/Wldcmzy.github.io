---
title: python 简单的颜色序列生成器.md
date: 1111-11-11 11:11:11
categories:
  - [教练我想学挂边躲牛, 杂乱]
tags:
  - OldBlog(Before20220505)
  - 画图
---

## 说明 - 2022-05-05
本篇博客为本人原创, 原发布于CSDN, 在搭建个人博客后使用爬虫批量爬取并挂到个人博客, 出于一些技术原因博客未能完全还原到初始版本(而且我懒得修改), 在观看体验上会有一些瑕疵 ,若有需求会发布重制版总结性新博客。发布时间统一定为1111年11月11日。钦此。

## 2021/04/21：我火星了？？？？

python Seaborn库 调色板

## 所以下面的东西都别看了

## .

## .

## .

## .

## .

## .

## .

## .

## .

## .

## 由来

昨天画了这张图，自动分配的颜色比较深，而且总颜色数不多，这张图的颜色是自己一点一点写的，很麻烦，所以想制作一个简单的颜色序列生成器一劳永逸  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210209193159338.png?x-oss-
process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dsZGNtenk=,size_16,color_FFFFFF,t_70#pic_center)

## 介绍

我给这个文件取名为InterestingColorfulColor.py，把它放在python安装目录的Lib文件夹就可以随时用import语句导入，非常方便。  
例如：


​    
```python
import InterestingColorfulColor as ICC
```

目前这个文件的主要内容只有一个类：ColorOrder  
ColorOrder类里给出了15种按照颜色深浅梯度变化的基础颜色序列，分别为：
**红，红橙，橙，橙黄，黄，黄绿，绿，绿青，青，青蓝，蓝，蓝紫，紫，紫红，和灰色** （由纯黑到纯白）  
每种序列有16种不同的颜色。  
**通过这个类可以较为轻松地得到一种你想要的颜色序列** ，有可能可以让你的工作提高一点点效率。不过这毕竟是个小制作，功能不复杂。  
下图展示其中一部分颜色  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210209193935836.png?x-oss-
process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dsZGNtenk=,size_16,color_FFFFFF,t_70#pic_center)

## 使用

#### 约定

约定上述15种颜色序列为 gray **red, orange,yellow,green,cyan,blue,purple**
以及除了gray外剩下七种颜色中相邻颜色之间的两两组合 如redorange,
greencyan等，对两种颜色书写的先后顺序不做要求即redpurple和purplered都是正确的

#### 导入模块


​    
```python
import InterestingColorfulColor as ICC
```


#### 创建一个ColorOrder对象


​    
```python
co = ICC.ColorOrder(['red','blue'],mixtype = 'cross')
```

参数解析：  
def **init** (self, colornames, mixtype = ‘Connect’)  
**两个参数，colornames ， mixtype**

**参数colornamse给出若干个颜色序列的名称。**  
**参数mixtype表示混合这些颜色序列的方式，目前只有两个值：connect 和 cross** （不需要区分大小写）  
connect为默认值 以连接的方式混合，即按照顺序依次把colornames中给出的颜色序列首尾相接  
cross 以交叉的方式混合 。举个例子 如果以cross的方式混合三个颜色序列[1,2,3][a,b,c][x,y,z]
最后的结果序列为[1,a,x,2,b,y,3,c,z]

#### 查看颜色序列，以及其他两个隐藏属性


​    
```python
co.getOrder() #查看序列
co.getLength() #查看序列长度
co.getBaseOrderNumber() #查看该序列由几个基础序列混合而成

结果：
['#2F0000', '#000079', '#4D0000', '#000093', '#600000', '#0000C6', '#750000', '#0000C6', '#930000', '#0000E3', '#AE0000', '#2828FF', '#CE0000', '#4A4AFF', '#EA0000', '#6A6AFF', '#FF0000', '#7D7DFF', '#FF2D2D', '#9393FF', '#FF5151', '#AAAAFF', '#ff7575', '#B9B9FF', '#FF9797', '#CECEFF', '#FFB5B5', '#DDDDFF', '#FFD2D2', '#ECECFF', '#FFECEC','#FBFBFF']
32
2
```


#### 切片获取序列


​    
```python
co.getLeft(False) #获得序列左边的25%
co.getRight(True) #获得序列右边的25%
co.getMiddle() #获得序列中间的50%
```

参数分析：  
def getLeft(self, muti_section = False):  
三个方法只有一个相同的参数，muti_section  
当muti_section默认为False 直接根据需求获得序列的前25%或后25%或中间50%  
当muti_section为True， 考虑某些时候切片以connect方式混合的序列，他会单独提取每个基础序列的左、右或中间的部分，拼接到一起作为返回结果

#### 与ColorOrder对象的序列混合


​    
```python
co.mix(co,'cross')
```

参数解析：  
def mix(self, co, mixtype = ‘Connect’)  
co为实例对象，mixtype与__init__中的mixtype相同  
值得一提的是这时两个序列不一定时等长的，我们再举一个cross混合的例子。假设混合[2,3,4,2,3,4,2,3,4]和[0,1,0,1,0,1]两个序列，最后的结果是[2,3,4,0,1,2,3,4,0,1,2,3,4,0
1]

## 应用简例


​    
```python
%matplotlib inline
import InterestingColorfulColor as ICC
import matplotlib.pyplot as plt

fig, ax = plt.subplots(1,2,figsize = (16,8))

co = ICC.ColorOrder(['blue','redpurple'],'cross')
ax[0].pie([1]*len(co.getOrder()),colors = co.getOrder())

co.mix(ICC.ColorOrder(['Green']),'cross')
ax[1].pie([1]*len(co.getMiddle()),colors = co.getMiddle())
```


效果：  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210209204037167.png?x-oss-
process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dsZGNtenk=,size_16,color_FFFFFF,t_70#pic_center)

## %……&%￥……？？？？？！！！

弄到颜色序列之后是可以用co.getOrder()导出来自己随便切的，并不是只能按照getMiddle,getLeft,getRight的固定方式切片

## 源代码


​    
```python
class ICC_UnknowColorName(Exception):
    pass
class ICC_UnknowMixType(Exception):
    pass

class ColorOrder:
    BaseOrderLength_Normal = 16
    #BaseOrderLength_Dull   = 12
    ErrorText_ICC_UnknowColorName = 'Exist color name not in ColorOrder.ColorMap, can not identify what color the name means. Or you need to use an iteratable object to iteratable object all color names'
    ErrorText_ICC_UnknowMixType = 'Can not identify mixtype'
    ColorMap = { 
         'Gray'        : 'Gray'
        ,'Red'         : 'Red'
        ,'Redpurple'   : 'RedPurple'
        ,'Purplered'   : 'RedPurple'
        ,'Purple'      : 'Purple'
        ,'Purpleblue'  : 'PurpleBlue'
        ,'Bluepurple'  : 'PurpleBlue'
        ,'Blue'        : 'Blue'
        ,'Bluecyan'    : 'BlueCyan'
        ,'Cyanblue'    : 'BlueCyan'
        ,'Cyan'        : 'Cyan'
        ,'Cyangreen'   : 'CyanGreen'
        ,'Greencyan'   : 'CyanGreen'
        ,'Green'       : 'Green'
        ,'Greenyellow' : 'GreenYellow'
        ,'Yellowgreen' : 'GreenYellow'
        ,'Yellow'      : 'Yellow'
        ,'Yelloworange': 'YellowOrange'
        ,'Orangeyellow': 'YellowOrange'
        ,'Orange'      : 'Orange'
        ,'Orangered'   : 'OrangeRed'
        ,'Redorange'   : 'OrangeRed'
        #,'Dullred'     : 'DullRed'
        #,'Dullyellow'  : 'DullYellow'
        #,'Dullcyan'    : 'DullCyan'
        #,'Dullblue'    : 'DullBlue'
        #,'Dullpurple'  : 'DullPurple'
    }
    BaseColorOrder = { 
         'Gray'        :   ['#ffffff','#272727','#3C3C3C','#4F4F4F','#5B5B5B','#6C6C6C','#7B7B7B','#8E8E8E','#9D9D9D','#ADADAD','#BEBEBE','#d0d0d0','#E0E0E0','#F0F0F0','#FCFCFC','#FFFFFF']
        ,'Red'         :   ['#2F0000','#4D0000','#600000','#750000','#930000','#AE0000','#CE0000','#EA0000','#FF0000','#FF2D2D','#FF5151','#ff7575','#FF9797','#FFB5B5','#FFD2D2','#FFECEC']
        ,'RedPurple'   :   ['#600030','#820041','#9F0050','#BF0060','#D9006C','#F00078','#FF0080','#FF359A','#FF60AF','#FF79BC','#FF95CA','#ffaad5','#FFC1E0','#FFD9EC','#FFECF5','#FFF7FB']
        ,'Purple'      :   ['#460046','#5E005E','#750075','#930093','#AE00AE','#D200D2','#E800E8','#FF00FF','#FF44FF','#FF77FF','#FF8EFF','#ffa6ff','#FFBFFF','#FFD0FF','#FFE6FF','#FFF7FF']
        ,'PurpleBlue'  :   ['#28004D','#3A006F','#4B0091','#5B00AE','#6F00D2','#8600FF','#921AFF','#9F35FF','#B15BFF','#BE77FF','#CA8EFF','#d3a4ff','#DCB5FF','#E6CAFF','#F1E1FF','#FAF4FF']
        ,'Blue'        :   ['#000079','#000093','#0000C6','#0000C6','#0000E3','#2828FF','#4A4AFF','#6A6AFF','#7D7DFF','#9393FF','#AAAAFF','#B9B9FF','#CECEFF','#DDDDFF','#ECECFF','#FBFBFF']
        ,'BlueCyan'    :   ['#000079','#003D79','#004B97','#005AB5','#0066CC','#0072E3','#0080FF','#2894FF','#46A3FF','#66B3FF','#84C1FF','#97CBFF','#ACD6FF','#C4E1FF','#D2E9FF','#ECF5FF']
        ,'Cyan'        :   ['#003E3E','#005757','#007979','#009393','#00AEAE','#00CACA','#00E3E3','#00FFFF','#4DFFFF','#80FFFF','#A6FFFF','#BBFFFF','#CAFFFF','#D9FFFF','#ECFFFF','#FDFFFF']
        ,'CyanGreen'   :   ['#006030','#01814A','#019858','#01B468','#02C874','#02DF82','#02F78E','#1AFD9C','#4EFEB3','#7AFEC6','#96FED1','#ADFEDC','#C1FFE4','#D7FFEE','#E8FFF5','#FBFFFD']
        ,'Green'       :   ['#006000','#007500','#009100','#00A600','#00BB00','#00DB00','#00EC00','#28FF28','#53FF53','#79FF79','#93FF93','#A6FFA6','#BBFFBB','#CEFFCE','#DFFFDF','#F0FFF0']
        ,'GreenYellow' :   ['#467500','#548C00','#64A600','#73BF00','#82D900','#8CEA00','#9AFF02','#A8FF24','#B7FF4A','#C2FF68','#CCFF80','#D3FF93','#DEFFAC','#E8FFC4','#EFFFD7','#F5FFE8']
        ,'Yellow'      :   ['#424200','#5B5B00','#737300','#8C8C00','#A6A600','#C4C400','#E1E100','#F9F900','#FFFF37','#FFFF6F','#FFFF93','#FFFFAA','#FFFFB9','#FFFFCE','#FFFFDF','#FFFFF4']
        ,'YellowOrange':   ['#5B4B00','#796400','#977C00','#AE8F00','#C6A300','#D9B300','#EAC100','#FFD306','#FFDC35','#FFE153','#FFE66F','#FFED97','#FFF0AC','#FFF4C1','#FFF8D7','#FFFCEC']
        ,'Orange'      :   ['#844200','#9F5000','#BB5E00','#D26900','#EA7500','#FF8000','#FF9224','#FFA042','#FFAF60','#FFBB77','#FFC78E','#FFD1A4','#FFDCB9','#FFE4CA','#FFEEDD','#FFFAF4']
        ,'OrangeRed'   :   ['#642100','#842B00','#A23400','#BB3D00','#D94600','#F75000','#FF5809','#FF8040','#FF8F59','#FF9D6F','#FFAD86','#FFBD9D','#FFCBB3','#FFDAC8','#FFE6D9','#FFF3EE']
        #,'DullRed'     :   ['#613030','#743A3A','#804040','#984B4B','#AD5A5A','#B87070','#C48888','#CF9E9E','#D9B3B3','#E1C4C4','#EBD6D6','#F2E6E6']
        #,'DullYellow'  :   ['#616130','#707038','#808040','#949449','#A5A552','#AFAF61','#B9B973','#C2C287','#CDCD9A','#D6D6AD','#DEDEBE','#E8E8D0']
        #,'DullCyan'    :   ['#336666','#3D7878','#408080','#4F9D9D','#5CADAD','#6FB7B7','#81C0C0','#95CACA','#A3D1D1','#B3D9D9','#C4E1E1','#D1E9E9']
        #,'DullBlue'    :   ['#484891','#5151A2','#5A5AAD','#7373B9','#8080C0','#9999CC','#A6A6D2','#B8B8DC','#C7C7E2','#D8D8EB','#E6E6F2','#F3F3FA']
        #,'DullPurple'  :   ['#6C3365','#7E3D76','#8F4586','#9F4D95','#AE57A4','#B766AD','#C07AB8','#CA8EC2','#D2A2CC','#DAB1D5','#E2C2DE','#EBD3E8']
      }
    
    def __init__(self, colornames, mixtype = 'Connect'):
        colornames = [i.capitalize() for i in colornames]
        mixtype = mixtype.capitalize()
        for each in colornames:
            if each not in ColorOrder.ColorMap:
                raise ICC_UnknowColorName(ColorOrder.ErrorText_ICC_UnknowColorName)
                
        self.__order = []
        if mixtype == 'Connect':
            for eachname in colornames:
                self.__order.extend(ColorOrder.BaseColorOrder[ColorOrder.ColorMap[eachname]])
        elif mixtype == 'Cross':
            lst = []
            for eachname in colornames:
                lst.append(ColorOrder.BaseColorOrder[ColorOrder.ColorMap[eachname]])
            for i in range(ColorOrder.BaseOrderLength_Normal):
                for each in lst:
                    self.__order.append(each[i])
        else:
            raise ICC_UnknowMixType(ColorOrder.ErrorText_ICC_UnknowMixType)
            
        self.__baseOrderNumber = len(colornames)
        self.__length = self.__baseOrderNumber * ColorOrder.BaseOrderLength_Normal
    
    def getBaseOrderNumber(self):
        return self.__baseOrderNumber
    
    def getLength(self):
        return self.__length
    
    def getOrder(self):
        return self.__order.copy()
    
    def getLeft(self, muti_section = False):
        if muti_section == False:
            lst = self.__order[ : 4 * self.__baseOrderNumber ]
        else:
            lst = []
            for i in range(self.__baseOrderNumber):
                pos = i * ColorOrder.BaseOrderLength_Normal
                lst.extend(self.__order[ pos : pos + 4])
        return lst
    
    def getRight(self, muti_section = False):
        if muti_section == False:
            lst = self.__order[ -4 * self.__baseOrderNumber : ]
        else:
            lst = []
            for i in range(1,self.__baseOrderNumber+1):
                pos = i * ColorOrder.BaseOrderLength_Normal
                lst.extend(self.__order[ pos - 4 : pos])
        return lst
    
    def getMiddle(self, muti_section = False):
        if muti_section == False:
            lst = self.__order[ 4 * self.__baseOrderNumber : 12 * self.__baseOrderNumber ]
        else:
            lst = []
            for i in range(self.__baseOrderNumber):
                pos = i * ColorOrder.BaseOrderLength_Normal
                lst.extend(self.__order[ pos + 4 : pos + 12])
        return lst
    
    def mix(self, co, mixtype = 'Connect'):
        mixtype = mixtype.capitalize()
        if mixtype == 'Connect':
            self.__order.extend(co.__order)
        elif mixtype == 'Cross':
            lst = []
            idx1 = 0
            idx2 = 0
            for i in range(ColorOrder.BaseOrderLength_Normal):
                for j in range(self.__baseOrderNumber):
                    lst.append(self.__order[idx1])
                    idx1 += 1
                for j in range(co.__baseOrderNumber):
                    lst.append(co.__order[idx2])
                    idx2 += 1
            self.__order = lst
        self.__length += co.__length
        self.__baseOrderNumber += co.__baseOrderNumber
```


​    

