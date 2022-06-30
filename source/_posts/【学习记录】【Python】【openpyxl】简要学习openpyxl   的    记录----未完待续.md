---
title: 【学习记录】【Python】【openpyxl】简要学习openpyxl   的    记录----未完待续.md
date: 1111-11-11 11:11:11
categories:
  - [基础知识, python, openpyxl]
tags:
  - OldBlog(Before20220505)
  - excle
  - python库
---

## 说明 - 2022-05-05
本篇博客为本人原创, 原发布于CSDN, 在搭建个人博客后使用爬虫批量爬取并挂到个人博客, 出于一些技术原因博客未能完全还原到初始版本(而且我懒得修改), 在观看体验上会有一些瑕疵 ,若有需求会发布重制版总结性新博客。发布时间统一定为1111年11月11日。钦此。



现阶段进行了简单的了解，不打算深究(后期来补: pandas挺好用的)

## 1.创建一个xls文件，随便写点东西，并保存


​    
    import openpyxl
    
    wb  = openpyxl.Workbook()   #创建一个工作表
    
    ws1 = wb.active             #获取“活跃的”表单
    ws1.title='sheeeet1'         #更改表单的名字
    
    ws1['B7']=15                           #给单元格赋值
    ws1['C4']='CFour'
    ws1['A2']='ttt'
    ws1['A8']='AaAaAaAa'
    ws1.append([1,2,'sfaf',56,'ABC'])       #给一行中的多个单元格赋值
    ws1.append([1,1,1,11,1,1,1,1])
    
    print(ws1.max_row)                      #最大行
    print(ws1.max_column)                   #最大列
    
    print(ws1['A2'].value)              
    print(ws1['B14'].value)
    print(ws1['B13'].value)                 #单元格无数据返回None


​    
    ws2 = wb.create_sheet('sheeeet2')       #创建一个表单
    ws2['C3']='ws2C3'


​    
    wb.save('xls_try1.xls')                 #保存为xls文件


​    

目测openpyxl.worksheet.worksheet.Worksheet类的append方法的添加位置是：最后有内容的一行 的下一行  
即openpyxl.worksheet.worksheet.Worksheet.max_row的下一行

## 2.打开一个xls文件，随便进行点操作


​    
    import openpyxl
    
    wb = openpyxl.load_workbook('xls_try1.xlsx')
    
    ws_one = wb['sheeeet1']
    print(ws_one['A2'].value)       #输出一个单元格的值
    
    for each_row in ws_one['A9':'F10']:
        for each_cell in each_row:
            print(each_cell.value)      #输出多个单元格的值

