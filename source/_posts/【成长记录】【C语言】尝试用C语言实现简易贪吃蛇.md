---
title: 【成长记录】【C语言】尝试用C语言实现简易贪吃蛇.md
date: 1111-11-11 11:11:11
categories:
  - [教练我想学挂边躲牛, 杂乱]
tags:
  - OldBlog(Before20220505)
---

## 说明 - 2022-05-05
本篇博客为本人原创, 原发布于CSDN, 在搭建个人博客后使用爬虫批量爬取并挂到个人博客, 出于一些技术原因博客未能完全还原到初始版本(而且我懒得修改), 在观看体验上会有一些瑕疵 ,若有需求会发布重制版总结性新博客。发布时间统一定为1111年11月11日。钦此。

刚开始查阅资料看别人的贪吃蛇代码，看得一头雾水，比如：  
SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE), c);  
这是个什么玩意儿？  
于是只能先去查阅一些资料。

# 1.技术支持

在稍微了解了控制台API函数后，发现这一堆巨长的字母也就是这么回事，虽然没背过，但拿过来用还是可以的。

**移动光标的函数**  
个人感觉这个算比较重要的


​    
    void Gotoxy(int x, int y)
    {
        COORD position = { y, x };
        SetConsoleCursorPosition(GetStdHandle(STD_OUTPUT_HANDLE), position);
    }


**更改字体颜色的函数**  
这个可以不要，纯粹是为了好看


​    
    int color(int c)
    {
    	//SetConsoleTextAttribute是API设置控制台窗口字体颜色和背景色的函数
    	SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE), c);       
    	return 0;
    }


**隐藏光标的函数**  
在制作的过程中发现光标一闪一闪的很碍眼，最初的操作是在不需要在地图上打印东西时把光标移动到（0，0），但发现有时候光标还是会在地图内闪烁，于是加入了这个函数（这里对我来说有一个问题，会在“遗留问题部分说明”）


​    
    void cursor_visible(int size,int visible)
    {
        CONSOLE_CURSOR_INFO cursor_info;
        cursor_info.bVisible=visible;
        cursor_info.dwSize=size;
        SetConsoleCursorInfo(GetStdHandle(STD_OUTPUT_HANDLE), &cursor_info);
    }


**其他**  
笔者自己的理解：  
fflush(stdin);清空缓冲区  
system(“cls”);刷屏  
_kbhit() 相应键盘输入  
getch() 接收字符

# 2.前期设定

设置一些基本的要素，如一些宏定义，全局变量，蛇与食物的结构体，函数声明，下面的代码中有一些时一开始就写上的，有一些是边写边加的（做完之后发现相较于最初的设想，改了好多东西）。  
**我把它们写在了自己的头文件head.h里：**


​    
    #ifndef HEAD_H_INCLUDED
    #define HEAD_H_INCLUDED
    #define TALL 30//地图高度
    #define DBW 68//地图宽度
    #define SNAKE_MAXLEN 500//蛇的极限长度（用于定义数组）
    #define SNAKE_INI_LEN 4
    #define SNAKE_INI_DELAY 200
    void cursor_visible(int,int);
    int color(int);
    void Gotoxy(int,int);
    void welcome(void);
    void printboard(void);
    void printnotes(void);
    void printsnake(void);
    void printfood(void);
    void snakemove(void);
    int jgameover(void);
    void gameover(int);
    int jeat(void);
    void repr_info(void);
    int score;
    struct
    {
        int x[SNAKE_MAXLEN];
        int y[SNAKE_MAXLEN];
        int delay;
        int len;//目前蛇的长度
        int dire;//direction蛇的移动方向
    }snake;
    struct
    {
        int x;
        int y;
    }food;
    #endif // HEAD_H_INCLUDED


看别人写的贪吃蛇都没有用数组来记录蛇的位置，（猜）可能是因为数组会占用连续的空间，这样不太好。  
（2020.04.15更新：他们都用的链表，当时我都没听过这东西）

# 3.着手实现

**显示最初界面的函数**


​    
    void welcome(void)
    {
        color(6);
        Gotoxy(3,10);
        printf("欢迎来到真·终极蛇皮上帝视角之皮死人不偿命之来回咕畜之贪吃蛇！");
        Gotoxy(6,10);
        printf("不温馨的提示：请先把游戏窗口调整至\"足够大\"，否则后果自负");
        Gotoxy(9,10);
        printf("按E可赛艇...  我是指按任意键开始游戏。");
        Gotoxy(12,10);
        color(7);
        system("pause");
    }


效果图  
![效果图](https://img-blog.csdnimg.cn/2020020420582644.png?x-oss-
process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dsZGNtenk=,size_16,color_FFFFFF,t_70)  
**画地图**  
发现代码写得好粗啊，浪费资源


​    
    void printboard(void)
    {
        int i,j;
        system("cls");
        color(9);
        for(i=0;i<=TALL;i++)
            for(j=0;j<=DBW;j+=2)
                if(i==0||i==TALL||j==0||j==DBW)
                {
                    Gotoxy(i,j);
                    printf("■");
                }
        color(7);
    }


**显示地图之外的一些信息**


​    
    void printnotes(void)
    {
        color(2);
        Gotoxy(3,74);
        printf("当前得分：%d",score);
        Gotoxy(4,74);
        printf("当前蛇长：%d节",SNAKE_INI_LEN);
        Gotoxy(8,74);
        printf("当前难度：较为平和");
        Gotoxy(9,74);
        printf("等待延迟：%d    ms（最小为20ms）",SNAKE_INI_DELAY);
        //Gotoxy(12,74);
        //printf("历史最高分为：");
        color(7);
    }


**画静态蛇**


​    
    void printsnake(void)
    {
        snake.x[0]=TALL/2;
        snake.y[0]=DBW/2;
        snake.delay=SNAKE_INI_DELAY;
        snake.len=SNAKE_INI_LEN;
        snake.dire='w';
        Gotoxy(snake.x[0],snake.y[0]);
        printf("⊙");
        for(int i=1;i<4;i++)
        {
            snake.x[i]=snake.x[i-1];
            snake.y[i]=snake.y[i-1]+2;
            Gotoxy(snake.x[i],snake.y[i]);
            printf("■");
        }


**随机生成食物**  
要通过一些措施保证食物在地图之内，而且还不能出现在蛇的身上。（这里也存在一个问题，会在之后的“遗留问题”部分说明。）  
需要注意一个细节，‘■’会横向占用两个字节，所以要让它们统一在偶数坐标生成


​    
    void printfood(void)
    {
        food.x=rand()%(TALL-2)+1;
        food.y=rand()%(DBW-4)+2;
        if(food.y%2)
            food.y++;
        int notput=1;
        while(notput)
        {
            for(int i=0;i<snake.len;i++)
            {
                if(!(food.x==snake.x[i]&&food.y==snake.y[i]))
                {
                    Gotoxy(food.x,food.y);
                    printf("奥");
                    notput=0;
                    break;
                }
            }
        }
    }


**蛇的移动**  
思路是先记录蛇最后一节的位置，用“
”（两个空格）覆盖蛇尾，在蛇的移动方向的下一个位置画蛇头，如果蛇吃到了食物，则在擦去的地方给蛇补充一节，一些相应的信息也要作改变  
查阅资料得到了_kbhit()和getch()的用法，_kbhit()可以响应键盘输入（大概），getch()可以接收字符，由于作者的了解也不深入，所以不多说。  
PS:隐藏光标的操作是后来加上的，导致某些移动光标的操作纯属多余


​    
    void snakemove(void)
    {
        int lastx=snake.x[snake.len-1],lasty=snake.y[snake.len-1];
        if(_kbhit())
        {
            fflush(stdin);
            snake.dire=getch();
        }
        for(int i=snake.len-1;i>0;i--)
        {
            snake.x[i]=snake.x[i-1];
            snake.y[i]=snake.y[i-1];
        }
        switch(snake.dire)
        {
        case 'w':
        case 'W':
            snake.x[0]-=1;
            break;
        case 'S':
        case 's':
            snake.x[0]+=1;
            break;
        case 'A':
        case 'a':
            snake.y[0]-=2;
            break;
        case 'D':
        case 'd':
            snake.y[0]+=2;
            break;
        }
        Gotoxy(snake.x[1],snake.y[1]);
        printf("■");
        Gotoxy(snake.x[0],snake.y[0]);
        printf("⊙");
        if(jeat())
        {
            score++;
            if(snake.len<SNAKE_MAXLEN)
                snake.len++;
            if(snake.delay>=22)
                snake.delay-=2;
            snake.x[snake.len-1]=lastx;
            snake.y[snake.len-1]=lasty;
            printfood();
            repr_info();
        }
        else
        {
            Gotoxy(lastx,lasty);
            printf("  ");
        }
        Gotoxy(0,0);
    }


**在蛇吃到食物后，改变一些显示信息**  
reprint information


​    
    void repr_info(void)
    {
        Gotoxy(3,84);
        color(2);
        printf("%d",score);
        Gotoxy(4,84);
        printf("%d",snake.len);
        Gotoxy(8,84);
        if(snake.delay>=170)
            printf("较为平和");
        else if(snake.delay>=120)
            printf("略高一筹");
        else if(snake.delay>=90)
            printf("考验手速");
        else if(snake.delay>=50)
            printf("高能预警");
        else if(snake.delay>=22)
            printf("终极蛇皮");
        else
            printf("MAX     ");
        Gotoxy(9,84);
        printf("%d  ",snake.delay);
        color(7);
    }


**检验是否吃到食物**  
judge eat


​    
    int jeat(void)
    {
        if(food.x==snake.x[0]&&food.y==snake.y[0])
            return 1;
        return 0;
    }


**检验蛇是否死亡**


​    
    int jgameover(void)
    {
        if(snake.x[0]==TALL||snake.x[0]==0||snake.y[0]==0||snake.y[0]==DBW)
            return 1;
        for(int i=1;i<snake.len;i++)
        {
            if(snake.x[0]==snake.x[i]&&snake.y[0]==snake.y[i])
                return 2;
        }
        return 0;
    }


**打印死亡后显示的一些东西**


​    
    void gameover(int j)
    {
        Gotoxy(20,75);
        printf("Game over\n");
        Gotoxy(21,73);
        if(j==1)
            printf("死因：铁齿铜牙与头铁");
        else if(j==2)
            printf("死因：我吃我自己");
    }


# 以下是所有代码

## 头文件


​    
    #ifndef HEAD_H_INCLUDED
    #define HEAD_H_INCLUDED
    #define TALL 30//地图高度
    #define DBW 68//地图宽度
    #define SNAKE_MAXLEN 500//蛇的极限长度（用于定义数组）
    #define SNAKE_INI_LEN 4
    #define SNAKE_INI_DELAY 200
    void cursor_visible(int,int);
    int color(int);
    void Gotoxy(int,int);
    void welcome(void);
    void printboard(void);
    void printnotes(void);
    void printsnake(void);
    void printfood(void);
    void snakemove(void);
    int jgameover(void);
    void gameover(int);
    int jeat(void);
    void repr_info(void);
    int score;
    struct
    {
        int x[SNAKE_MAXLEN];
        int y[SNAKE_MAXLEN];
        int delay;
        int len;//目前蛇的长度
        int dire;//direction蛇的移动方向
    }snake;
    struct
    {
        int x;
        int y;
    }food;


​    
    #endif // HEAD_H_INCLUDED


​    
​    

## 源文件

主函数单独写了


​    
    #include <stdio.h>
    #include <stdlib.h>
    #include <windows.h>
    #include <unistd.h>
    #include <time.h>
    #include <string.h>
    #include "head.h"
    void cursor_visible(int size,int visible)
    {
        CONSOLE_CURSOR_INFO cursor_info;
        cursor_info.bVisible=visible;
        cursor_info.dwSize=size;
        SetConsoleCursorInfo(GetStdHandle(STD_OUTPUT_HANDLE), &cursor_info);
    }
    int color(int c)
    {
    	//SetConsoleTextAttribute是API设置控制台窗口字体颜色和背景色的函数
    	SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE), c);
    	return 0;
    }
    void Gotoxy(int x, int y)
    {
        COORD position = { y, x };
        SetConsoleCursorPosition(GetStdHandle(STD_OUTPUT_HANDLE), position);
    }
    void welcome(void)
    {
        color(6);
        Gotoxy(3,10);
        printf("欢迎来到真·终极蛇皮上帝视角之皮死人不偿命之来回咕畜之贪吃蛇！");
        Gotoxy(6,10);
        printf("不温馨的提示：请先把游戏窗口调整至\"足够大\"，否则后果自负");
        Gotoxy(9,10);
        printf("按E可赛艇...  我是指按任意键开始游戏。");
        Gotoxy(12,10);
        color(7);
        system("pause");
    }
    void printboard(void)
    {
        int i,j;
        system("cls");
        color(9);
        for(i=0;i<=TALL;i++)
            for(j=0;j<=DBW;j+=2)
                if(i==0||i==TALL||j==0||j==DBW)
                {
                    Gotoxy(i,j);
                    printf("■");
                }
        color(7);
    }
    void printnotes(void)
    {
        color(2);
        Gotoxy(3,74);
        printf("当前得分：%d",score);
        Gotoxy(4,74);
        printf("当前蛇长：%d节",SNAKE_INI_LEN);
        Gotoxy(8,74);
        printf("当前难度：较为平和");
        Gotoxy(9,74);
        printf("等待延迟：%d    ms（最小为20ms）",SNAKE_INI_DELAY);
        //Gotoxy(12,74);
        //printf("历史最高分为：");
        color(7);
    }
    void printsnake(void)
    {
        snake.x[0]=TALL/2;
        snake.y[0]=DBW/2;
        snake.delay=SNAKE_INI_DELAY;
        snake.len=SNAKE_INI_LEN;
        snake.dire='w';
        Gotoxy(snake.x[0],snake.y[0]);
        printf("⊙");
        for(int i=1;i<4;i++)
        {
            snake.x[i]=snake.x[i-1];
            snake.y[i]=snake.y[i-1]+2;
            Gotoxy(snake.x[i],snake.y[i]);
            printf("■");
        }
    
    }
    void printfood(void)
    {
        food.x=rand()%(TALL-2)+1;
        food.y=rand()%(DBW-4)+2;
        if(food.y%2)
            food.y++;
        int notput=1;
        while(notput)
        {
            for(int i=0;i<snake.len;i++)
            {
                if(!(food.x==snake.x[i]&&food.y==snake.y[i]))
                {
                    Gotoxy(food.x,food.y);
                    printf("奥");
                    notput=0;
                    break;
                }
            }
        }
    }
    void snakemove(void)
    {
        int lastx=snake.x[snake.len-1],lasty=snake.y[snake.len-1];
        if(_kbhit())
        {
            fflush(stdin);
            snake.dire=getch();
        }
        for(int i=snake.len-1;i>0;i--)
        {
            snake.x[i]=snake.x[i-1];
            snake.y[i]=snake.y[i-1];
        }
        switch(snake.dire)
        {
        case 'w':
        case 'W':
            snake.x[0]-=1;
            break;
        case 'S':
        case 's':
            snake.x[0]+=1;
            break;
        case 'A':
        case 'a':
            snake.y[0]-=2;
            break;
        case 'D':
        case 'd':
            snake.y[0]+=2;
            break;
        }
        Gotoxy(snake.x[1],snake.y[1]);
        printf("■");
        Gotoxy(snake.x[0],snake.y[0]);
        printf("⊙");
        if(jeat())
        {
            score++;
            if(snake.len<SNAKE_MAXLEN)
                snake.len++;
            if(snake.delay>=22)
                snake.delay-=2;
            snake.x[snake.len-1]=lastx;
            snake.y[snake.len-1]=lasty;
            printfood();
            repr_info();
        }
        else
        {
            Gotoxy(lastx,lasty);
            printf("  ");
        }
        Gotoxy(0,0);
    }
    int jgameover(void)
    {
        if(snake.x[0]==TALL||snake.x[0]==0||snake.y[0]==0||snake.y[0]==DBW)
            return 1;
        for(int i=1;i<snake.len;i++)
        {
            if(snake.x[0]==snake.x[i]&&snake.y[0]==snake.y[i])
                return 2;
        }
        return 0;
    }
    void gameover(int j)
    {
        Gotoxy(20,75);
        printf("Game over\n");
        Gotoxy(21,73);
        if(j==1)
            printf("死因：铁齿铜牙与头铁");
        else if(j==2)
            printf("死因：我吃我自己");
    }
    int jeat(void)
    {
        if(food.x==snake.x[0]&&food.y==snake.y[0])
            return 1;
        return 0;
    }
    void repr_info(void)
    {
        Gotoxy(3,84);
        color(2);
        printf("%d",score);
        Gotoxy(4,84);
        printf("%d",snake.len);
        Gotoxy(8,84);
        if(snake.delay>=170)
            printf("较为平和");
        else if(snake.delay>=120)
            printf("略高一筹");
        else if(snake.delay>=90)
            printf("考验手速");
        else if(snake.delay>=50)
            printf("高能预警");
        else if(snake.delay>=22)
            printf("终极蛇皮");
        else
            printf("MAX     ");
        Gotoxy(9,84);
        printf("%d  ",snake.delay);
        color(7);
    }


​    

## 主函数


​    
    int main()
    {
        cursor_visible(25,0);
        score=0;
        srand(time(NULL));
        welcome();
        printboard();
        printnotes();
        printsnake();
        printfood();
        while(1)
        {
            int j;
            snakemove();
            if(j=jgameover())
            {
                gameover(j);
                break;
            }
            Sleep(snake.delay);
        }
        Gotoxy(TALL+1,0);
        cursor_visible(25,1);
        system("pause");
        return 0;
    }


​    

游戏效果图  
![游戏效果图](https://img-blog.csdnimg.cn/20200204215133333.png?x-oss-
process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dsZGNtenk=,size_16,color_FFFFFF,t_70)

# 遗留问题

**1.关于随机生成食物（程序问题）**  
笔者认为已经对食物的出现未知作出了判断，但在实际操作过程中还是会出现食物生成在蛇身上的状况

**2.关于光标隐藏（知识盲点）**  
笔者最早的光标移动函数是这么写的


​    
    void cursor_visible(int visible)
    {
        CONSOLE_CURSOR_INFO cursor_info;
        cursor_info.bVisible=visible;
        SetConsoleCursorInfo(GetStdHandle(STD_OUTPUT_HANDLE), &cursor_info);
    }


但发现通过cursor_visible(0);这个操作并不能隐藏光标。  
本着一本正经乱改的心态改成了这样


​    
    void cursor_visible(int size,int visible)
    {
        CONSOLE_CURSOR_INFO cursor_info;
        cursor_info.bVisible=visible;
        cursor_info.dwSize=size;
        SetConsoleCursorInfo(GetStdHandle(STD_OUTPUT_HANDLE), &cursor_info);
    }


笔者认为调节光标尺寸应该与是否隐藏光标没什么关系，然而事实却是即使使用光标的默认尺寸25（即不对光标尺寸做更改），通过cursor_visible(25，0);也可以隐藏光标。

**3.当快速连续有效输入（程序问题）**  
当快速连续按下方向键，比如“sdsdsdsdsdsdsdsdsdsdsd”，（猜）可能有大量的字符留在缓冲区，使蛇“延迟”，不听操作，但笔者暂时不会修正这个bug。

**4.蛇可以直接反向撞自己导致死亡**  
可能需要加入一些限制，比如snake.dire=‘w’时不能直接反向改变其值为’s’

**5.可能还存在众多未发现的问题**

