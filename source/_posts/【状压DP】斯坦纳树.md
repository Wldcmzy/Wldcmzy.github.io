---
title: 【状压DP】斯坦纳树.md
date: 1111-11-11 11:11:11
categories:
  - OldBlog(Before20220505)
tags:
  - OldBlog(Before20220505)
---

## 说明 - 2022-05-05
本篇博客为本人原创, 原发布于CSDN, 在搭建个人博客后使用爬虫批量爬取并挂到个人博客, 出于一些技术原因博客未能完全还原到初始版本(而且我懒得修改), 在观看体验上会有一些瑕疵 ,若有需求会发布重制版总结性新博客。发布时间统一定为1111年11月11日。钦此。

## 前言

斯坦纳树常用于解决类似这样的最小代价联通问题：<https://www.luogu.com.cn/problem/P4294>

## 思路

**tip1:**  
对于一个结点, 通过状态压缩的思想，用一个整数sta表示这个结点与所有目标节点的联通状态，
在sta的二进制位中，若第x位为1，表示该结点与第x个目标结点联通，反之则为不连通  
用dp[i][sta]表示结点i联通sta状态的最小代价

**状态转移part1**  
若结点i与状态sta1和sta2都联通，且sta1与sta2无重合结点(即sta1, sta2
是对sta1+sta2的一个划分)。令P表示把点i联通所需的代价， 显然有:  
dp[i][sta1 + sta2] = MIN(dp[i][sta1 + sta2], dp[i][sta1] + dp[i][sta2] - P[i])  
由于dp[i][sta1] 和dp[i][sta2]都包含了联通点i的代价P[i]，所以要减去一个P[i]。  
dp[i][sta1]中的[i]只是一种结点的表示方法.  
**状态转移part2**  
按照边进行松弛  
对于一个结点i， dp[i][sta] = MIN(dp[i][sta], dp[i周围一步可达的结点][sta] + P[i])  
像极了最短路， 直接spfa

## 题目

#### P4294 [WC2008]游览计划

<https://www.luogu.com.cn/problem/P4294>

由于你还需要输出所有位置的摆放 所以加入了pre数组记录某状态的前置状态。得出最小答案后通过dfs找出斯坦纳树的所有结点。  
(对于状态转移part1, 只需要记录一个前置状态，另一个前置状态通过减法求出)


​    
```cpp
#include <bits/stdc++.h>
const int N = 13, LIM = 1028, INF = 0x3f3f3f3f;
int dx[] = {0, 0, 1, -1};
int dy[] = {1, -1, 0, 0};
int board[N][N], dp[N][N][LIM];
int n, m, idx, mSta, rex, rey;
bool vis[N][N], judge;
struct PRE{
    int x, y, s;
}pre[N][N][LIM];
void dfs(int x, int y, int st){
    vis[x][y] = true;
    PRE tmp = pre[x][y][st];
    if(!tmp.x && !tmp.y) return ;
    dfs(tmp.x, tmp.y, tmp.s);
    if(tmp.x == x && tmp.y == y) dfs(x, y, st - tmp.s);
}
inline bool cango(int x, int y) {return x<=n && y<=m && x>=1 && y>=1; }
int main(){
    memset(dp, 63, sizeof(dp));
    scanf("%d%d", &n, &m);
    for(int i=1; i<=n; i++) for(int j=1; j<=m; j++) {
        scanf("%d", &board[i][j]);
        if(!board[i][j]) dp[i][j][1 << idx ++] = 0;
    }
    mSta = 1 << idx;
    for(int sta=1; sta<mSta; sta++){
        std::queue<std::pair<int ,int> > q;
        for(int i=1; i<=n; i++) for(int j=1; j<=m; j++){
            for(int sub=sta; sub; sub=(sub-1)&sta){
                if(dp[i][j][sub] + dp[i][j][sta - sub] - board[i][j] < dp[i][j][sta]){
                    dp[i][j][sta] = dp[i][j][sub] + dp[i][j][sta - sub] - board[i][j];
                    pre[i][j][sta] = (PRE){i, j, sub};
                }
            }
            if(dp[i][j][sta] < INF) q.push(std::make_pair(i, j)), vis[i][j] = true;
        }
        while(!q.empty()){
            std::pair<int , int> x = q.front(); q.pop(); vis[x.first][x.second] = false;
            for(int i=0; i<4; i++){
                int xx = x.first + dx[i], yy = x.second + dy[i];
                if(cango(xx, yy)) if(dp[x.first][x.second][sta] + board[xx][yy] < dp[xx][yy][sta]){
                    dp[xx][yy][sta] = dp[x.first][x.second][sta] + board[xx][yy];
                    pre[xx][yy][sta] = (PRE){x.first, x.second, sta};
                    if(!vis[xx][yy]) vis[xx][yy] = true, q.push(std::make_pair(xx, yy));
                }
            }
        }
    }
    for(int i=1; i<=n && !judge; i++) for(int j=1; j<=m && !judge; j++) if(!board[i][j]) rex = i, rey = j, judge = true;
    printf("%d\n", dp[rex][rey][mSta - 1]);
    dfs(rex, rey, mSta - 1);
    for(int i=1; i<=n; i++) {
        for(int j=1; j<=m; j++){
            if(board[i][j] == 0) putchar('x');
            else if(vis[i][j] == 1) putchar('o');
            else putchar('_');
        }
        putchar('\n');
    }
    return 0;
}
```

