---
title: 【学习记录】【python】【tkinter】自学tkinter的简要记录.md
date: 1111-11-11 11:11:11
categories:
  - OldBlog(Before20220505)
tags:
  - OldBlog(Before20220505)
---

## 说明 - 2022-05-05
本篇博客为本人原创, 原发布于CSDN, 在搭建个人博客后使用爬虫批量爬取并挂到个人博客, 出于一些技术原因博客未能完全还原到初始版本(而且我懒得修改), 在观看体验上会有一些瑕疵 ,若有需求会发布重制版总结性新博客。发布时间统一定为1111年11月11日。钦此。

## 0\. tkinter.Tk（） 零基础建议从1开始看别从0

主窗口的一些方法


​    
```python
import tkinter as tk

root=tk.Tk()

def exit():
    root.quit()                 #退出
def update():
    root.update()               #刷新界面

root.title('标题名称就叫这个')
root.geometry('250x150')        #主窗体的大小
root.resizable(20,0)             #框体大小可调性，分别表示x,y方向的可变性,第一个参数为x，第二个为y
                                
bt1=tk.Button(root,text='exit',command=exit)
bt1.pack(side=tk.BOTTOM)
bt2=tk.Button(root,text='update',command=update)
bt2.pack(side=tk.BOTTOM)

tk.mainloop()
```


## 1\. Lable Button

显示时间


​    
```python
from tkinter import *
import time

#给button按钮用的函数
def refresh_time():
    timestring.set('%s'%time.ctime())
    complexLabel.update()

root=Tk()

frame1=Frame(root)
frame1.pack()
frame2=Frame(root)
frame2.pack()
```


​    
```python
#Label
timestring=StringVar()              #建立StringVar类对象
timestring.set('%s'%time.ctime())   #初始化StringVar对象的值
img=PhotoImage(file='theimg.png')   #建立PhotoImage类对象
complexLabel=Label(frame1,
                #text='天王盖地虎，\n小鸡炖蘑菇。',  #文本
                textvariable=timestring,            #可变文本
                image=img,                          #图像

                justify=RIGHT,      #对齐方式 每行文字都会向这个方向对齐，默认CENTER居中
                anchor=None,          #文字在Label框内的方位 N,S,E,W,NE,NW,SE,SW(东南西北)
                compound=CENTER,    #组合方式 LEFT图左文右，RIGHT图右文左，CENTER图文重合

                height=360,width=640,   #Label的宽高  单位：以系统默认的中文的一个字体宽高为单位
                padx=20,pady=10,        #边距  单位是像素
                fg='blue',bg=None,      #颜色  fg：字体颜色，bg：字体背景色
                font=('华文行楷',35),    #字体名+大小

                relief='raised',         #边框样式 flat,sunken,raised,ridge
                bd=15                    #边框大小
                )
complexLabel.pack()

#Button
button=Button(frame2,
            text='刷新时间',
            image=None,         #Button的大小根据图片大小确定

            justify=None,   
            anchor=None,          
            compound=None,      #这些在Button里同样适用    

            font=('华文行楷',15),   
            fg='red',bg=None,
            width=20,height=1,
            padx=10,pady=5,

            bd=8,
            relief='sunken',    #flat groove ridge raised solid sunken

            cursor='hand2' ,    #鼠标  有pencil,circle,hand1,hand2

            command=refresh_time  #点击时触发函数 传参数用lambda:函数(参数)的形式
            )
button.pack()

mainloop()
```


​    

效果图  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200501165700545.PNG?x-oss-
process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dsZGNtenk=,size_16,color_FFFFFF,t_70)

## 2\. Checkbutton Raidobutton

随便敲了个屑作


​    
```python
from tkinter import *

root=Tk()

v1=IntVar()
v2=IntVar()
v3=IntVar()
var=IntVar()

frame1=LabelFrame(root,bg='brown',text='Checkbutton部分',labelanchor=NE)
frame1.place(relx=0.2,rely=0.2,relwidth=0.3,relheight=0.3)
frame2=LabelFrame(root,bg='green',text='Raidobutton部分',labelanchor=NW)
frame2.place(relx=0.5,rely=0.5,relwidth=0.4,relheight=0.4)
```


​    
​    
```python
check1=Checkbutton(frame1,text='流浪汉',variable=v1,fg='blue',bg='green')
check1.grid(row=0,column=0)
check2=Checkbutton(frame1,text='高中生',variable=v2,fg='black',bg='green')
check2.grid(row=1,column=0)
check3=Checkbutton(frame1,text='穿越者',variable=v3,fg='red',bg='green')
check3.grid(row=2,column=0)
```


​    
​    
```python
radio1=Radiobutton(frame2,text='流浪汉',variable=var,value=1)
radio1.grid(row=0,column=0)
radio2=Radiobutton(frame2,text='掠夺者',variable=var,value=1)
radio2.grid(row=0,column=1)
radio3=Radiobutton(frame2,text='高中生',variable=var,value=2)
radio3.grid(row=1,column=0)
radio4=Radiobutton(frame2,text='初中生',variable=var,value=2)
radio4.grid(row=1,column=1)
radio5=Radiobutton(frame2,text='穿越者',variable=var,value=3)
radio5.grid(row=2,column=0)
radio6=Radiobutton(frame2,text='龙傲天',variable=var,value=3)
radio6.grid(row=2,column=1)

mainloop()
```


​    

效果图  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200501165729478.PNG?x-oss-
process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dsZGNtenk=,size_16,color_FFFFFF,t_70)

## 3\. Entry 及复习前面

简易计算器


​    
```python
from tkinter import *

def changeop(value):
    oplst=['+','-','*','/']
    opstr.set(oplst[value])

def calcu():
    oplst=[lambda:v1.get()+v2.get(),lambda:v1.get()-v2.get(),lambda:v1.get()*v2.get(),lambda:v1.get()/v2.get()]
    try:
        v3.set(oplst[opvar.get()]())
        errstr.set('')
    except ZeroDivisionError:
        errstr.set('除零错误')
    except TclError:
        errstr.set('输入错误')
    except IndexError:
        errstr.set('无运算符')
    except:
        errstr.set('未知错误')
        
def clear():
    v1.set(0)
    v2.set(0)
    v3.set(0)
    opvar.set(5)
    opstr.set('')
    errstr.set('')
```


​    
```python
root=Tk()

#定义变量
v1=IntVar()
v2=IntVar()
v3=IntVar()
errstr=StringVar()
opstr=StringVar()
opvar=IntVar()
opvar.set(5)

frame=LabelFrame(root,text='计算器',labelanchor=NW)
frame.pack()

#三个数字
add1=Entry(frame,textvariable=v1,bg='lightgrey',width=8)
add1.grid(row=0,column=0)
add2=Entry(frame,textvariable=v2,bg='lightgrey',width=8)
add2.grid(row=0,column=2)
result=Label(frame,textvariable=v3,bg='lightgrey',width=8,anchor=W)
result.grid(row=0,column=4)

#运算符和等号
ch1=Label(frame,textvariable=opstr)
ch1.grid(row=0,column=1)
ch2=Label(frame,text='=')
ch2.grid(row=0,column=3)

#错误提示
errlabel=Label(frame,textvariable=errstr,fg='red')
errlabel.grid(row=0,column=5)

#运算符选项
op1=Radiobutton(frame,text='+',value=0,variable=opvar,command=lambda:changeop(opvar.get()))
op1.grid(row=1,column=0)
op1=Radiobutton(frame,text='-',value=1,variable=opvar,command=lambda:changeop(opvar.get()))
op1.grid(row=1,column=1)
op1=Radiobutton(frame,text='*',value=2,variable=opvar,command=lambda:changeop(opvar.get()))
op1.grid(row=1,column=2)
op1=Radiobutton(frame,text='/',value=3,variable=opvar,command=lambda:changeop(opvar.get()))
op1.grid(row=1,column=3)

#两个按钮
btcalcu=Button(frame,text='计算',command=calcu,width=8)
btcalcu.grid(row=1,column=4)
btclear=Button(frame,text='清除',command=clear,width=8)
btclear.grid(row=1,column=5)
```


​    
```python
mainloop()
```


​    

效果图  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200501165758245.PNG)

## 4\. Listbox Scrollbar 少量Text

Listbox Scrollbar Text 的简单应用


​    
```python
import tkinter as tk
root=tk.Tk()

lst=['幽灵弯刀','学术剽窃','苔丝格雷迈恩','迈拉的不稳定元素','弑君','蜡烛巨龙','团伙劫掠','阿卡玛','灵魂之境',
    '炎魔之王拉格纳罗斯','光耀之主拉格纳罗斯','索瑞森大帝','厄运信天翁','露娜的口袋银河','观星者露娜','拉祖尔',
    '火车王埃利诺','卡扎库斯','了不起的杰弗里斯','铜须布莱恩','伊利斯逐星','雷诺杰克逊','芬利忘了爵士']
#可以用for循环添加一些项目
var=tk.StringVar()
#StringVar类变量存放Listbox中的所有项目，以空格隔开
var.set('1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20')

frame1=tk.LabelFrame(root,bg='lightgrey',text='Listbox',labelanchor=tk.NW)
frame1.pack(side=tk.LEFT)
frame2=tk.LabelFrame(root,bg='lightgrey',text='Text',labelanchor=tk.NW)
frame2.pack(side=tk.RIGHT)

scrbar=tk.Scrollbar(frame1,command=None)    #仅仅这样用户拖动滚动条是不能实现Listbox的上下移动的，下文通过config方法改变了command的值
scrbar.pack(side=tk.LEFT,fill=tk.Y)        
```


​    
```python
lstbx=tk.Listbox(frame1,
                height=8,                   #显示8行
                fg='blue',
                yscrollcommand=scrbar.set,  #设置y方向滚动条

                selectbackground='green',    #选中时的背景色
                selectforeground='red',      #选中时的前景色
                selectborderwidth=5,        #选中时的边框宽度 默认是由selectbackground指定的颜色填充，没有边框 如果设置了此选项，Listbox 的每一项会相应变大，被选中项为 "raised" 样式
                
                selectmode=tk.SINGLE,       #选择模式 SINGLE 单选 BROWSE 单选，可鼠标和方向键改变选项
                                            #       MULTIPLE 多选 EXTENDED 多选，需要配合shift crtl 或拖动光标实现
                
                cursor='hand2',             #鼠标选在Listbox上的样式，pencil,circle,hand1,hand2

                exportselection=False,      #指定文本是否可以被复制到剪贴板

                listvariable=var            #StringVar类变量存放Listbox中的所有项目，以空格隔开
                )
lstbx.pack(side=tk.LEFT,fill=tk.BOTH)

for i in lst:
    lstbx.insert(tk.END,i)

scrbar.config(command=lstbx.yview)          #Listbox组件的yview方法用于在垂直方向上滚动 Listbox 组件的内容
                                            
#Listbox的方法以及Scrollbar的参数和方法 这里没有记录

def add():
    text.insert(tk.END,lstbx.get(tk.ACTIVE)+'\n')   #在Text的末尾-END位置-写入Listbox的选中项

button1=tk.Button(frame1,text='删除',command=lambda x=lstbx:x.delete(tk.ACTIVE))    #Listbox.delete(tk.ACTIVE)删除选中
button1.pack(side=tk.BOTTOM)
button2=tk.Button(frame1,text='加入',command=add)
button2.pack(side=tk.BOTTOM)

text=tk.Text(frame2)
text.pack()

tk.mainloop()
```


​    

Listbox的方法以及Scrollbar的参数和方法有很多没有记录，也暂时没有学习，学习的时候找到这位了博主发过的一些比较详细的资料  
<https://blog.csdn.net/qq_41556318/article/list>

效果图  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200501165914516.PNG?x-oss-
process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dsZGNtenk=,size_16,color_FFFFFF,t_70)

## 5\. Text 之 mark与tag

关于mark的简单知识


​    
```python
import tkinter as tk

root=tk.Tk()

scrbar=tk.Scrollbar(root)
scrbar.pack(side=tk.LEFT,fill=tk.Y)

text1=tk.Text(root,width=50,height=15)
text1.pack(side=tk.LEFT)
text2=tk.Text(root,width=50,height=15)
text2.pack(side=tk.RIGHT)

text1.config(yscrollcommand=scrbar.set) #关联text1和滚动条
scrbar.config(command=text1.yview)
```


​    
```python
text1.insert(tk.INSERT,'在Insert位置插入这段话+换行\n')     #汉字只占一列的位置
text2.insert(tk.INSERT,'在Insert位置插入这段话+换行\n')

text1.mark_set('thismark','1.5')        #创建一个新mark，若mark存在，则是移动mark到指定位置
text2.mark_set('thismark','1.5')        

text2.mark_gravity('thismark',tk.LEFT)  #体会gravity为LEFT和RIGHT的不同(默认是RGHIT)
                                        #insert操作之后mark会保持在插入内容的gravity的方向

text1.insert('thismark','(第一Giao)')
text1.insert('thismark','(第二Giao)')
text2.insert('thismark','(第一Giao)')
text2.insert('thismark','(第二Giao)')
```


​    
```python
text1.insert(tk.END,'\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n')
text1.insert(tk.END,text1.mark_names())                
#mark_names()方法返回Text的所有mark(返回tuple类型) 包括INSERT CURRENT 注意END不是mark是特殊索引

text1.insert(tk.END,'\n'+text1.mark_next(0.1)+'\n')     #mark_next()方法返回index位置后的第一个mark，若没有，返回None
text1.insert(tk.END,type(text1.mark_previous(0.1)))     #mark_previous()方法返回index位置前的第一个mark，若没有，返回None

text1.mark_unset('thismark')
text2.mark_unset('thismark')        #封印mark

tk.mainloop()
```


​    

效果图  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200502141505911.PNG?x-oss-
process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dsZGNtenk=,size_16,color_FFFFFF,t_70)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200502141505910.PNG?x-oss-
process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dsZGNtenk=,size_16,color_FFFFFF,t_70)

关于tag的简单知识


​    
```python
import tkinter as tk

root=tk.Tk()

text=tk.Text(root,width=50,height=15)
text.pack()

text.insert(tk.INSERT,'abdcefghijklmnopqrstuvwxyz')

text.tag_config(tk.SEL,background='black',foreground='white')   #SEL是预定义的特殊tag，表示对应的选中内容
```


​    
```python
text.tag_add('thistag','1.5','1.9','1.14','1.18','1.0','1.1','1.20') #连续的两个参数表示范围，一个表示一个位置
text.tag_config('thistag',background='lightgrey',foreground='green')

text.tag_raise(tk.SEL)      
#新建tag会覆盖较为旧的tag的相同选项
#用tag_raise()或tag_lower()可以调整tag优先级
```


​    
```python
tk.mainloop()
```


效果图

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020050214152292.PNG?x-oss-
process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dsZGNtenk=,size_16,color_FFFFFF,t_70)

## 6\. Canvas

暂时不想学，放在这里吃灰吧

## 7\. Menu 相关的一些简单知识

简单实现了 Menu Menubotton OptionMenu 组件


​    
```python
import tkinter as tk

root=tk.Tk()

menu=tk.Menu(root,tearoff=False)        #建立一个Menu menu  rearoff=true表示这个菜单可“撕下”(单独变成一个小窗口)
root.config(menu=menu)

filemenu=tk.Menu(menu,tearoff=False)    #建立另一个Menu filemenu
filemenu.add_command(label='打开')
filemenu.add_command(label='保存')
filemenu.add_separator()                #一道线
filemenu.add_command(label='退出',command=root.quit)
menu.add_cascade(label='文件',menu=filemenu)    #添加一个父菜单

frame=tk.Frame(root,width=500,height=200)
frame.pack()
def popup(event):
    filemenu.post(event.x_root,event.y_root)

frame.bind('<Button-3>',popup)              #通过绑定事件（右击）使filemenu出现
```

```python
#--------------------------------------------------------------------------------------------------
#Menubutton
menubutton=tk.Menubutton(root,text='click me',relief=tk.RAISED)
menubutton.pack()

second_filemenu=tk.Menu(menubutton)
second_filemenu.add_command(label='giaogiao')
menubutton.config(menu=second_filemenu)
```

```python
#--------------------------------------------------------------------------------------------------
#OptionMenu（M大写）
options=['624','625','626','010','1437','4369','777']
var=tk.StringVar()
var.set(options[0])
optmenu=tk.OptionMenu(root,var,*options) 
optmenu.pack()
```

```python
tk.mainloop()
```


​    

效果图  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200502191209144.PNG?x-oss-
process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dsZGNtenk=,size_16,color_FFFFFF,t_70)

## 8\. Message Spinbox简单知识拓展

Message使Label的变体，支持多行文本消息和自动  
Spinbox是Entry的变体， 从一些固定值中选择一个，可以通过 1.范围 2.元组 指定内容


​    
​    import tkinter as tk
​    
    root=tk.Tk()
    
    frame1=tk.LabelFrame(root,text='message',labelanchor=tk.NW)
    frame1.pack(side=tk.LEFT)
    frame2=tk.LabelFrame(root,text='spinbox',labelanchor=tk.NW)
    frame2.pack(side='right')
    
    #Message使Label的变体，支持多行文本消息和自动
    msg1=tk.Message(frame1,text='我是一个massage',width=150)
    msg1.pack()
    msg2=tk.Message(frame1,text='我是一个巨巨巨巨巨巨巨巨巨巨巨巨巨巨巨长的message',width=200)
    msg2.pack()
    
    #Spinbox是Entry的变体， 从一些固定值中选择一个，可以通过 1.范围 2.元组 指定内容
    #1.指定范围
    spb1=tk.Spinbox(frame2,from_=0,to=15)
    spb1.pack(side=tk.LEFT)
    #2.指定元组
    lst=['奥雷利亚诺布恩迪亚','何塞阿尔卡蒂奥布恩迪亚','乌尔苏拉','梅尔基亚德斯','蕾梅黛丝','赫里内勒多马尔克斯','丽贝卡','阿马兰妲','加西亚马尔克斯']
    spb2=tk.Spinbox(frame2,values=lst)
    spb2.pack()
    
    tk.mainloop()


效果图  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200503160621198.PNG)

## 9\. Toplevel

实现顶级窗口


​    
​    import tkinter as tk
​    
    root=tk.Tk()
    root.title('root里面放按钮')
    num=0
    
    def create():
        global num
        num+=1
    
        top=tk.Toplevel()         #创建Top窗口
        top.title('第'+str(num)+'窗')
        top.attributes('-alpha',0.7)    #设置窗口不透明度为0.7
        
        msg=tk.Message(top,text='第%d个窗口成功创建!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!'%num,width=300)
        msg.pack()


​    
​    tk.Button(root,text='创建一个Toplevel窗口',width=70,command=create,relief=tk.RAISED,bd=10).pack()
​    
    tk.mainloop()


效果图  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200503163759839.PNG?x-oss-
process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dsZGNtenk=,size_16,color_FFFFFF,t_70)  
在学习过程中，发现这位博主有关于Toplevel 和Tk 方法，以及attributes函数 的较详细内容  
<https://blog.csdn.net/sinat_41104353/article/details/79320155>

## 10\. 事件绑定 简单知识

可以把事件和组件或窗口（可能表述会有问题）绑定起来以形如  
窗口.bind(事件序列,函数)的形式，回调函数时会带着Event（事件）对象作为参数

事件序列的语法描述为<modifier-type-detail>（举个栗子<Control-Shift-
KeyPress-h>，表示同时ctrl+shift+小写h）详情见下面链接

学习时发现其他博主发的资料：  
事件序列：<https://blog.csdn.net/nkd50000/article/details/77319659>  
Event对象的属性及含义：<https://blog.csdn.net/nkd50000/article/details/77319760>

。  
简单试水


​    
​    import tkinter as tk
​    
    lst=[
        '<Button-1>',
        '<Control-Button-1>',
        '<Control-Shift-Button-3>',
        '<Control-Shift-KeyPress-Q>',
        '<Alt-KeyPress-N>',
        '<Alt_L>',
        '<Double-Button-1>',
        '<Shift_L>',
        '<Double-KeyPress-A>',
        '<Triple-KeyPress>',
        '<Shift-KeyPress-H>',
        #'<Any-KeyPress>'
        ]
    
    root=tk.Tk()
    
    frame=tk.Frame(root,width=250,height=50,bg='blue')
    frame.pack()
    frame.focus_set()
    
    def function(event):
        top=tk.Toplevel()
        tk.Message(top,text=event.type).pack()
    
    for each in lst:
        frame.bind(each,function)
    #frame.bind('<KeyPress>',function)
    #frame.bind('<KeyPress-Y>',function)


​    
​    
​    tk.mainloop()


## 11\. 标准对话框

massage简单实现


​    
​    import tkinter.messagebox as tkmsg
​    import tkinter as tk


​    
​    root=tk.Tk()
​    
    v=tk.StringVar()
    lb=tk.Message(root,textvariable=v)
    lb.pack()
    
    #返回 字符串 yes no
    res=tkmsg.askquestion(
                    'askquestion',
                    '随便写点什么12',
                    default=tkmsg.NO,
                    #default定义按下回车的默认选项（默认值为一个按钮）
                    #CANCEL IGNORE OK NO RETRY YES
                    icon=tkmsg.ERROR,
                    #icon 指定框内显示的图标，一般不要乱指定，用默认
                    #ERROR INFO QUESTION WARNNING
                    
                    #对话框默认显示在根窗口，通过parent参数 如parent=w  可以显示在子窗口
                    ) 
    v.set(res)
    
    #返回 bool值 True False
    res=tkmsg.askyesno('askyesno','随便写点什么3244231423')
    v.set(res)
    res=tkmsg.askretrycancel('askretrycancel','随便写点什么3454345')
    v.set(res)
    res=tkmsg.askokcancel('askokcancel','随便写点什么56765')
    v.set(res)
    
    #返回 字符串 ok
    res=tkmsg.showerror('showerror','随便写点什么rtfdx')
    v.set(res)
    res=tkmsg.showinfo('showinfo','随便写点什么3r4eds')
    v.set(res)
    res=tkmsg.showwarning('showwarning','随便写点什么d gfgdsgr')
    v.set(res)


​    
​    tk.mainloop()


filedialog模块

> 导入模块：import tkinter.filedialog  
>  选择文件对话框的格式：  
>  tkinter.filedialog.asksaveasfilename():选择以什么文件名保存，返回文件名  
>  tkinter.filedialog.asksaveasfile():选择以什么文件保存，创建文件并返回文件流对象  
>  tkinter.filedialog.askopenfilename():选择打开什么文件，返回文件名  
>  tkinter.filedialog.askopenfile():选择打开什么文件，返回IO流对象  
>  tkinter.filedialog.askdirectory():选择目录，返回目录名  
>  tkinter.filedialog.askopenfilenames():选择打开多个文件，以元组形式返回多个文件名  
>  tkinter.filedialog.askopenfiles():选择打开多个文件，以列表形式返回多个IO流对象

## 12\. 布局管理器

举个栗子


​    
​    import tkinter as tk
​    
    root=tk.Tk()
    
    frame1=tk.Frame(root,width=200,height=150,bg='lightgrey')
    frame1.pack(side=tk.LEFT,fill=tk.Y)
    frame2=tk.Frame(root,width=200,height=150,bg='yellow')
    frame2.pack(side=tk.LEFT)
    frame3=tk.Frame(root,width=200,height=150,bg='purple')
    frame3.pack(side=tk.RIGHT,fill=tk.BOTH)
    
    lb1=tk.Label(frame1,text='123123',bg='green')
    lb1.place(relx=0.1,rely=0.1,relwidth=0.4,relheight=0.4)
    
    lb2=tk.Label(frame2,text=369396,bg='blue')
    lb2.place(relx=0.1,rely=0.1,relwidth=0.8,relheight=0.8)


​    
​    bt1=tk.Button(frame3,text='aoe')
​    bt1.grid(row=0,column=0)
​    bt2=tk.Button(frame3,text='iuv')
​    bt2.grid(row=1,column=1)
​    
    tk.mainloop()


## 少量拾遗

tkinter.Text.yview_moveto(1)移至最低端

tkinter.Text的state参数为’disabled’时不允许修改 state的默认参数为’normal’

