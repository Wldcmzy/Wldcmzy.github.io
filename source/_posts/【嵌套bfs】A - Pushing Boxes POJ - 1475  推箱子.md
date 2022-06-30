---
title: 【嵌套bfs】A - Pushing Boxes POJ - 1475  推箱子.md
date: 1111-11-11 11:11:11
categories:
  - [算法, 搜索]
tags:
  - OldBlog(Before20220505)
  - BFS
---

## 说明 - 2022-05-05
本篇博客为本人原创, 原发布于CSDN, 在搭建个人博客后使用爬虫批量爬取并挂到个人博客, 出于一些技术原因博客未能完全还原到初始版本(而且我懒得修改), 在观看体验上会有一些瑕疵 ,若有需求会发布重制版总结性新博客。发布时间统一定为1111年11月11日。钦此。

https://vjudge.net/contest/387870#problem/A

## 题目描述

Imagine you are standing inside a two-dimensional maze composed of square
cells which may or may not be filled with rock. You can move north, south,
east or west one cell at a step. These moves are called walks.  
One of the empty cells contains a box which can be moved to an adjacent free
cell by standing next to the box and then moving in the direction of the box.
Such a move is called a push. The box cannot be moved in any other way than by
pushing, which means that if you push it into a corner you can never get it
out of the corner again.

One of the empty cells is marked as the target cell. Your job is to bring the
box to the target cell by a sequence of walks and pushes. As the box is very
heavy, you would like to minimize the number of pushes. Can you write a
program that will work out the best such sequence?  
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly92ai56MTgwLmNuLzk2MTQ4YTA0MzgzZDlmZTI2MzBhM2FjNzcyZjEyMjYx?x-oss-
process=image/format,png#pic_center)

## 输入

The input contains the descriptions of several mazes. Each maze description
starts with a line containing two integers r and c (both <= 20) representing
the number of rows and columns of the maze.

Following this are r lines each containing c characters. Each character
describes one cell of the maze. A cell full of rock is indicated by a `#' and
an empty cell is represented by a`.’. Your starting position is symbolized by
`S', the starting position of the box by`B’ and the target cell by `T’.

Input is terminated by two zeroes for r and c.

## 输出

For each maze in the input, first print the number of the maze, as shown in
the sample output. Then, if it is impossible to bring the box to the target
cell, print ``Impossible.’’.

Otherwise, output a sequence that minimizes the number of pushes. If there is
more than one such sequence, choose the one that minimizes the number of total
moves (walks and pushes). If there is still more than one such sequence, any
one is acceptable.

Print the sequence as a string of the characters N, S, E, W, n, s, e and w
where uppercase letters stand for pushes, lowercase letters stand for walks
and the different letters stand for the directions north, south, east and
west.

Output a single blank line after each test case.

## 样例输入

1 7  
SB…T  
1 7  
SB…#.T  
7 11  
###########  
#T##…#  
#.#.#…####  
#…B…#  
#.######…#  
#…S…#  
###########  
8 4  
…  
.##.  
.#…  
.#…  
.#.B  
.##S  
…  
###T  
0 0

## 样例输出

Maze #1  
EEEEE

Maze #2  
Impossible.

Maze #3  
eennwwWWWWeeeeeesswwwwwwwnNN

Maze #4  
swwwnnnnnneeesssSSS

## 自己加一组样例输入

6 5  
…T  
.S…  
#B###  
…  
…  
…

## 自己加的样例的输出

Maze #1  
SSesswNNNNwnEEE

## 瞎翻译

这是一个推箱子游戏。人要把箱子推到终点，我们需要输出人走的路径（方向用nswe或NSWE表示，人不推箱子只走路时路径用小写字母nswe表示，人推箱子走路时路径用大写字母NSWE表示）

先输入两个整数row和col，表示地图大小

之后输入一个row行col列的char类型矩阵，其中#代表墙，.代表路，S代表人的起始位置，B代表箱子起始位置，T代表终点（箱子要被推到的位置）

## 题解

**1.**  
先设想箱子会自己动，想象一下如何用简单的bfs求得“箱子自己动”情况下的答案  
**2.**  
在”箱子自己动“的代码基础上加入人：  
假设箱子要往右动，那么先对人进行bfs，让人从自己的位置走到箱子的左边的位置，如果人可以走过去，则箱子可以往右移动。  
**2.5**  
记得人推箱子的时候人也要动，而不是仅仅箱子移动（人与箱子往相同方向移动，或者直接把人移动到箱子的上一个位置）。  
**3.**  
这是笔者自己犯的一个错误：  
不能仅仅用形如visb[][]的二维数组标记箱子到过的位置，因为在某些情况下人想推箱子到终点，是需要【箱子折返】的（箱子有可能去一个地方两次或多次，可以见我自己加的输入样例）  
所以要用三维数组：visb[][][4],第三维记录箱子来时的方向

**4.**  
在箱子移动的bfs函数中嵌套人移动的bfs函数完成求解


​    
    #include <cstdio>
    #include <iostream>
    #include <algorithm>
    #include <cstring>
    #include <queue>
    #include <string>
    
    int row,col,counter;
    const int maxn=25;
    char cell[maxn][maxn];
    bool visb[maxn][maxn][4],visp[maxn][maxn];  //visb记录箱子，visp记录人去过的位置
    //人的标记方式为：先在原地标记一次，之后去了哪标哪
    //箱子的标记方式为：不在原地标记，只有从一个方向（假设为2）移动到某一格（假设为i，j），则标记visb[i][j][2]为true
    
    int step[4][2]={{1,0},{-1,0},{0,1},{0,-1}};
    char adir[5]="snew",Adir[5]="SNEW";         //两个dir都对应step的四个方向
    struct Pos
    {
        int x,y;            //表示位置，若是人，只会用到这两个和path
        int mx,my;          //表示人的位置，表示箱子的变量用的，表示人的变量用不着
        std::string path;   //记录路径
    }mpre,mnxt,tar,bpre,bnxt,to;
    //mpre-man pre人的当前位置（或解释为人的上一个位置）mnxt-man next人的下一个位置
    //bpre和bnxt表示箱子的当前位置和下一个位置
    //tar表示终点位置
    //to表示箱子要移动到某个位置时，人要移动到的位置


​    
​    
    //判断这个点是否在地图可行走范围之中
    bool cango(int x,int y)
    {
        if(x>=0 && x<row && y>=0 && y<col && cell[x][y]!='#')
            return true;
        return false;
    }
    
    //人从自己所在的位置，移动到to.x,to.y的位置，简单bfs
    bool bfsMan()
    {
        memset(visp,false,sizeof(visp));
        std::queue<Pos> q;
        mpre.path="";
        mpre.x=bnxt.mx,mpre.y=bnxt.my;
        q.push(mpre);
        visp[mpre.x][mpre.y]=true;
        while(!q.empty())
        {
            mpre=q.front(); q.pop();
            if(mpre.x==to.x && mpre.y==to.y) return true;
            for(int i=0; i<4; i++)
            {
                mnxt=mpre;
                mnxt.x+=step[i][0],mnxt.y+=step[i][1];
                if(cango(mnxt.x,mnxt.y) && !visp[mnxt.x][mnxt.y] && !(mnxt.x==bpre.x && mnxt.y==bpre.y))
                {
                    visp[mnxt.x][mnxt.y]=true;
                    mnxt.path+=adir[i];
                    q.push(mnxt);
                }
            }
        }
        return false;
    
    }
    
    //对箱子的bfs，其中嵌套了对人的bfs
    void bfs()
    {
        std::queue<Pos> q;
        bpre.path="";
        bpre.mx=mpre.x,bpre.my=mpre.y;
        q.push(bpre);//让最初箱子的位置入队，记得给箱子结构体赋值人的位置
        while(!q.empty())
        {
            bpre=q.front(); q.pop();
            for(int i=0; i<4; i++)  //开始四个方向的遍历
            {
                bnxt=bpre;
                bnxt.x+=step[i][0],bnxt.y+=step[i][1];
                if(cango(bnxt.x,bnxt.y) && !visb[bnxt.x][bnxt.y][i])
                {
                    to=bpre;
                    to.x-=step[i][0],to.y-=step[i][1];
                    if(cango(to.x,to.y))    //初步检查这一点是否可以达到
                    {
                        if(bfsMan())
                        {
                            //一定要bfsMan（）返回true时才标记visb为true
                            //因为若人不能到达特定的推箱子位置，箱子就不能访问到要去的位置
                            visb[bnxt.x][bnxt.y][i]=true;
                            bnxt.path+=mpre.path;               //路径末尾加入人上一次移动的路径
                            bnxt.path+=Adir[i];                 //路径末尾加入箱子移动的路径
                            //如果到了终点，直接输出
                            if(bnxt.x==tar.x && bnxt.y==tar.y)
                            {
                                std::cout << "Maze #" << counter++ << std::endl;
                                std::cout << bnxt.path << std::endl << std::endl;
                                return ;
                            }
                            bnxt.mx=bpre.x,bnxt.my=bpre.y;      //记得记录箱子在这个位置时对应的人的位置，然后入队
                            q.push(bnxt);
                        }
                    }
                }
            }
        }
        std::cout << "Maze #" << counter++ << std::endl;
        std::cout << "Impossible." << std::endl << std::endl;
    }


​    
    int main()
    {
        counter=1;
        while(std::cin >> row >> col && !(row==0 && col==0))
        {
            memset(visb,false,sizeof(visb));
            for(int i=0; i<row; i++)
            {
                std::cin >> cell[i];
                for(int j=0; j<col; j++)
                {
                    if(cell[i][j]=='S')  mpre.x=i,mpre.y=j;
                    else if(cell[i][j]=='B') bpre.x=i,bpre.y=j;
                    else if(cell[i][j]=='T') tar.x=i,tar.y=j;
                }
            }
            bfs();
        }
    
        return 0;
    }


​    

