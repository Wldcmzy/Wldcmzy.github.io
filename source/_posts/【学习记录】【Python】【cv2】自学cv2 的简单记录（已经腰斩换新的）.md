---
title: 【学习记录】【Python】【cv2】自学cv2 的简单记录（已经腰斩换新的）.md
date: 1111-11-11 11:11:11
categories:
  - [基础知识, python, opencv]
tags:
  - OldBlog(Before20220505)
  - cv
  - 图像处理
  - python库
---

## 说明 - 2022-05-05
本篇博客为本人原创, 原发布于CSDN, 在搭建个人博客后使用爬虫批量爬取并挂到个人博客, 出于一些技术原因博客未能完全还原到初始版本(而且我懒得修改), 在观看体验上会有一些瑕疵 ,若有需求会发布重制版总结性新博客。发布时间统一定为1111年11月11日。钦此。

## 1.图像的载入、显示、保存


​    
​    import cv2
​    
    #读入图像 cv2.imread(filepath,flags)
    #flags参数的取值：
    #cv2.IMREAD_COLOR：默认，载入一个彩色图像，忽略透明度   可用1代替
    #cv2.IMREAD_GRAYSCALE：载入一个灰阶图像  可用0代替
    #cv2.IMREAD_UNCHANGED：载入一个包含 Alpha 通道（透明度）的图像   可用-1代替
    img1=cv2.imread('img_chess.jpg',0)
    
    #显示图像   cv2.imshow(wname,img)
    #wname  窗口的名字 window name
    #img 要显示的图像 窗口他大小为自动调整图片大小
    cv2.imshow('image_one',img1)
    key=cv2.waitKey(0)                  #等待键盘输入，单位毫秒，0为无限等待  没有这句话窗口只会闪一下就消失
    if key==27:
        print('您按了ESC')
    #cv2.destroyAllWindows()            #销毁所有窗口
    cv2.destroyWindow('image_one')      #指定窗口名字销毁窗口


​    
​    #保存图像 cv2.imwrite(file，img，num)
​    #file 文件名
​    #img  要保存的图像
​    #num 对于JPEG，其表示的是图像的质量，用0 - 100的整数表示，默认95;对于png 用0-9 ,第三个参数表示的是压缩级别。默认为3.
​    cv2.imwrite('img_chess_gray.jpg',img1,[cv2.IMWRITE_JPEG_QUALITY,0])
​    cv2.imwrite('img_chess_gray.png',img1,[cv2.IMWRITE_PNG_COMPRESSION,0])
​    # jpg属于有损压缩，是以图片的清晰度为代价的，数字越小，压缩比越高，图片质量损失越严重
​    # png属于无损压缩，数字0-9，数字越低，压缩比越低
​    
    input('按回车退出程序')


那张保存为jpg格式的图片惨不忍睹

## 2.简单的图片操作 反转、复制、缩放、裁剪

flip copy resize


​    
​    import cv2
​    
    img1=cv2.imread('img_chess.jpg')
    
    #图片反转 cv2.flip(img,flipcode)
    #flipcode >0 沿x轴 flipcode=0 沿y轴 flipcode<0 xy轴同时反转
    img1_flipx=cv2.flip(img1,1)
    img1_flipy=cv2.flip(img1,0)
    img1_flipxy=cv2.flip(img1,-1)
    
    cv2.imshow('1fx',img1_flipx)
    cv2.imshow('1fy',img1_flipy)
    cv2.imshow('1fxy',img1_flipxy)
    cv2.waitKey(0)
    cv2.destroyAllWindows()


​    
​    #图像复制
​    img1_cp=img1.copy()
​    cv2.imshow('1cp',img1_cp)
​    cv2.waitKey()
​    cv2.destroyAllWindows()


​    
​    #图像的缩放
​    img_info=img1.shape
​    height=img_info[0]
​    width=img_info[1]
​    img1_resized=cv2.resize(img1.copy(),(int(width*1.2),int(height*0.6)))
​    cv2.imshow('1resized',img1_resized)
​    cv2.waitKey(0)
​    cv2.destroyAllWindows()
​    
    #图像的裁剪
    img1_cut=img1[350:800,60:500]
    h,w,n=img1.shape
    img1_cut2=img1[int(h/4):int(h*3/4),int(w/4):int(w*3/4)]
    cv2.imshow('1cut',img1_cut)
    cv2.imshow('1cut2',img1_cut2)
    cv2.waitKey(0)
    cv2.destroyAllWindows()


## 3.图像通道的分离与合成

**分离：**  
img1B,img1G,img1R=np.dsplit(img1,3)  
img1B,img1G,img1R=cv2.split(img1)都可以  
**合成：**  
cv2.merge([b,g,r])  
cv2.merge([zeros, zeros, img1R])

**注意倒数第五句的注释**


​    
​    import cv2
​    import numpy as np
​    
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


​    
​    img1=cv2.imread('theimg.png',1)
​    
    img1B,img1G,img1R=np.dsplit(img1,3)
    #img1B,img1G,img1R=cv2.split(img1)
    
    print(img1B.dtype)          #矩阵元素的格式为uint8


​    
​    
​    showone(img1B)      #这里图像为灰色，原因是当调用cv2.imshow的时候G、R两个通道被默认设为了与B通道一样，三通道一样时显示灰度图


​    
​    showone_color(img1B,'B')
​    showone_color(img1G,'G')
​    showone_color(img1R,'R')
​    
    showone_merge(img1B,img1G,img1R)        #合成后是原图


​    

