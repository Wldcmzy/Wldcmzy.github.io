---
title: 【树的直径 dfs dp】P4408 [NOI2003] 逃学的小孩.md
date: 1111-11-11 11:11:11
categories:
  - [算法, 图论]
tags:
  - OldBlog(Before20220505)
  - DFS
  - DP
---

## 说明 - 2022-05-05
本篇博客为本人原创, 原发布于CSDN, 在搭建个人博客后使用爬虫批量爬取并挂到个人博客, 出于一些技术原因博客未能完全还原到初始版本(而且我懒得修改), 在观看体验上会有一些瑕疵 ,若有需求会发布重制版总结性新博客。发布时间统一定为1111年11月11日。钦此。

## P4408 [NOI2003] 逃学的小孩


​    
    #include <iostream>
    #include <cstdio>
    #include <cstdlib>
    #include <cstring>
    #include <cmath>
    #include <algorithm>
    #include <string>
    #include <queue>
    #include <vector>
    #include <stack>
    #include <map>
    #include <set>
    typedef long long int LL;
    const int N = 2e5+5, M = -1, INF = 0x7fffffff;
    int n,m,u,v,pid,tid;
    LL w,plong,tlong;
    int idx;
    int head[N];
    LL disa[N],disb[N];
    struct Edge
    {
        int to,nxt;
        LL w;
    }edg[N<<1];
    void addEdge(int fr,int to,LL w)
    {
        edg[idx].to = to;
        edg[idx].nxt = head[fr];
        edg[idx].w = w;
        head[fr] = idx++;
    }
    void initial()
    {
        idx = 2;
    }
    void dfs(LL leng,int id,int efrom)
    {
        if(leng > plong){
            plong = leng;
            pid = id;
        }
        for(int e=head[id]; e; e=edg[e].nxt){
            if((e^1) == efrom) continue;
            dfs(leng+edg[e].w,edg[e].to,e);
        }
    }
    void DFS(LL dis[],int x,int efrom,LL leng)
    {
        dis[x] = leng;
        for(int e=head[x]; e; e=edg[e].nxt){
            if((e^1) == efrom) continue;
            int y = edg[e].to;
            DFS(dis,y,e,leng+edg[e].w);
        }
    }
    int main()
    {
        initial();
        scanf("%d%d",&n,&m);
        for(int i=0; i<m; i++) {
            scanf("%d%d%lld",&u,&v,&w);
            addEdge(u,v,w);
            addEdge(v,u,w);
        }
        pid = 1, plong = 0;
        dfs(0,pid,0);
        tid = pid;
        pid = tid, plong = 0;
        dfs(0,pid,0);
        DFS(disa,tid,0,0);
        DFS(disb,pid,0,0);
        tlong = 0;
        for(int i=1; i<=n; i++){
            tlong = std::max(tlong,std::min(disa[i],disb[i]));
        }
        printf("%lld",tlong+plong);
        return 0;
    }

