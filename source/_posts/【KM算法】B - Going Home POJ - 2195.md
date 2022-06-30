---
title: 【KM算法】B - Going Home POJ - 2195.md
date: 1111-11-11 11:11:11
categories:
  - [算法, 图论]
tags:
  - 二分图
  - KM算法
  - OldBlog(Before20220505)
---

## 说明 - 2022-05-05
本篇博客为本人原创, 原发布于CSDN, 在搭建个人博客后使用爬虫批量爬取并挂到个人博客, 出于一些技术原因博客未能完全还原到初始版本(而且我懒得修改), 在观看体验上会有一些瑕疵 ,若有需求会发布重制版总结性新博客。发布时间统一定为1111年11月11日。钦此。

https://vjudge.net/contest/389071#problem/B

## 题目描述

On a grid map there are n little men and n houses. In each unit time, every
little man can move one unit step, either horizontally, or vertically, to an
adjacent point. For each little man, you need to pay a $1 travel fee for every
step he moves, until he enters a house. The task is complicated with the
restriction that each house can accommodate only one little man.

Your task is to compute the minimum amount of money you need to pay in order
to send these n little men into those n different houses. The input is a map
of the scenario, a ‘.’ means an empty space, an ‘H’ represents a house on that
point, and am ‘m’ indicates there is a little man on that point.

![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly92ai56MTgwLmNuLzI5ZGE0YzIyY2Q5YTM1ZWI5NmIyYTA4Yjc1Y2U4NDIw?x-oss-
process=image/format,png#pic_center)

You can think of each point on the grid map as a quite large square, so it can
hold n little men at the same time; also, it is okay if a little man steps on
a grid with a house without entering that house.

## 输入

There are one or more test cases in the input. Each case starts with a line
giving two integers N and M, where N is the number of rows of the map, and M
is the number of columns. The rest of the input will be N lines describing the
map. You may assume both N and M are between 2 and 100, inclusive. There will
be the same number of 'H’s and 'm’s on the map; and there will be at most 100
houses. Input will terminate with 0 0 for N and M.

## 输出

For each test case, output one line with the single integer, which is the
minimum amount, in dollars, you need to pay.

## 样例输入

2 2  
.m  
H.  
5 5  
HH…m  
…  
…  
…  
mm…H  
7 8  
…H…  
…H…  
…H…  
mmmHmmmm  
…H…  
…H…  
…H…  
0 0

## 样例输出

2  
10  
28

## 瞎翻译

给你一张地图，地图被分割成了N行M列的网格。

第一行：输入N，M表示地图大小。

第2 到 第N+1行： 输入地图内容， .代表空地，m代表人，H代表房子。

N和M不大于100，房子数目不大于100

人和房子的数量是相等的，而人要移动进入房子，且每个房子只能进一个人。  
不过人每移动一格（横移或者竖移）要花一块钱。

**问：**  
要使人都进入房子最少花多少钱？  
（地图的每一格都足够大，可以容纳无限多的人。）

## 题解

思路：  
**1.**  
地图最大为100x100 ，所以人最多花不到200元钱，我们定一个常数ML=210.  
**2.**  
先计算每个人到每个房子需要花的钱，用ML减去需要花的钱，得到的数值，存进数组map[][]（不是地图，我取的映射的意思）

这么存的原因是：KM算法可以算出最多要花多少钱，然而我们需要求最少要花多少钱，这样就只需要把“最小的”先暂时变成“最大的”，完成KM算法之后再额外通过一步计算得到正确答案。

推导可知：正确答案=（ML*房子数量）-KM算法得到的最大值


​    
​    #include <cstdio>
​    #include <iostream>
​    #include <algorithm>
​    #include <cstring>
​    
    const int maxn=110;
    const int ML=220;//花费不可能超过200
    char cell[maxn][maxn];
    int map[maxn][maxn],lx[maxn],ly[maxn],link[maxn];
    //map取的意思是映射，map[i][j]表示第i个人到第j个房子的距离
    //lx表示人顶标的值，ly表示房子顶标的值
    bool visx[maxn],visy[maxn];
    int pnum,ans;
    struct Node
    {
        int x,y;
    }person[maxn],house[maxn];
    
    bool dfs(int x)
    {
    
        visx[x]=true;
        for(int i=0; i<pnum ;i++)
        {
    //这里的lx[x]一开始写成了lx[i]，找bug找了半天，巨麻烦，打标记警醒一下
            if(!visy[i] && lx[x]+ly[i]==map[x][i])
            {
                visy[i]=true;
                if(link[i]==-1 || dfs(link[i]))
                {
                    link[i]=x;
                    return true;
                }
            }
        }
        return false;
    }
    
    int km()
    {
        memset(lx,0,sizeof(lx));
        memset(ly,0,sizeof(ly));
        memset(link,-1,sizeof(link));
        for(int i=0; i<pnum ; i++)
            for(int j=0; j<pnum; j++)
            {
                lx[i]=std::max(lx[i],map[i][j]);
            }
        for(int i=0; i<pnum; i++)
        {
            memset(visx,false,sizeof(visx));
            memset(visy,false,sizeof(visy));
            while(!dfs(i))
            {
                int tmp=ML;
    
    //--------------------------->>特此标记<<-----------------------------
    // KM没学好，这里一开始想错了，误认为下面tmp=min（）式子中lx的下标一定是i，
    //导致下方两行没要，一直错被卡了很长时间
    
                for(int j=0; j<pnum; j++)
                    if(visx[j])
    //*******************************************************************************
                        for(int k=0; k<pnum; k++)
                            if(visy[k]==false)
                                tmp=std::min(tmp,lx[j]+ly[k]-map[j][k]);
                for(int j=0; j<pnum; j++)
                {
                    if(visx[j])
                        lx[j]-=tmp;
                    if(visy[j])
                        ly[j]+=tmp;
                }
                memset(visx,false,sizeof(visx));
                memset(visy,false,sizeof(visy));
            }
        }
        ans=0;
        for(int i=0;i<pnum; i++)
        {
            ans+=lx[i]+ly[i];
        }
        return ML*pnum-ans;
    }
    
    int main()
    {
        int n,m;
        int p,h;
        while(std::cin >> n >> m && !(n==0 && m==0))
        {
            p=h=0;
            for(int i=0;i <n; i++)
                for(int j=0; j<m ; j++)
                {
                    std::cin >> cell[i][j];
                    if(cell[i][j]=='m')
                    {
                        person[p].x=i;
                        person[p++].y=j;
                    }
                    else if(cell[i][j]=='H')
                    {
                        house[h].x=i;
                        house[h++].y=j;
                    }
                }
            pnum=p;
            for(int i=0; i<p; i++)
                for(int j=0; j<h; j++)
                {
                    map[i][j]=abs(person[i].x-house[j].x)+abs(person[i].y-house[j].y);
                    map[i][j]=ML-map[i][j];
                }
            std::cout << km() << std::endl;
        }
    
        return 0;
    }


​    

