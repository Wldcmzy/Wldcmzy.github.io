---
title: 【单源最短路 spfa Dijkstra】 _ 洛谷 P3371 【模板】单源最短路径（弱化版）_ ## lguou.md
date: 1111-11-11 11:11:11
categories:
  - [算法, 图论]
tags:
  - BFS
---

## 说明 - 2022-05-05
本篇博客为本人原创, 原发布于CSDN, 在搭建个人博客后使用爬虫批量爬取并挂到个人博客, 出于一些技术原因博客未能完全还原到初始版本(而且我懒得修改), 在观看体验上会有一些瑕疵 ,若有需求会发布重制版总结性新博客。发布时间统一定为1111年11月11日。钦此。

## 洛谷 P3371 【模板】单源最短路径（弱化版）

Dijkstra


​    
    #include <cstdio>
    #include <iostream>
    #include <cstring>
    #include <cstdlib>
    #include <algorithm>
    #include <vector>
    #include <map>
    #include <set>
    #include <queue>
    #include <stack>
    typedef long long int LL;
    const int N = 1e4+5, M = 5e5+5,INF = 0x7fffffff;
    int n,m,u,v,d,s;
    int idx;
    int head[N],dis[N];
    bool vis[N];
    struct Edge
    {
       int to,nxt,d;
    }edg[M];
    void addEdge(int fr,int to,int d)
    {
       edg[idx].to = to;
       edg[idx].nxt = head[fr];
       edg[idx].d = d;
       head[fr] = idx++;
    }
    struct PPP
    {
       int val,dis;
       bool operator < (const PPP &x) const{
          return dis > x.dis;
       }
    };
    std::priority_queue<PPP> q;
    int main()
    {
       //freopen("D:\\EdgeDownloadPlace\\P3371_2.in","r",stdin);
       memset(head,-1,sizeof(head));
       for(int i=0;i<N; i++) dis[i] = INF;
       scanf("%d%d%d",&n,&m,&s);
       for(int i=1; i<=m; i++){
          scanf("%d%d%d",&u,&v,&d);
          addEdge(u,v,d);
       }
       dis[s] = 0;
       q.push((PPP){s,0});
       while(!q.empty()){
          PPP cur = q.top(); q.pop();
          int x = cur.val;
          if(vis[x]) continue;
          vis[x] = true;
          for(int e=head[x]; e!=-1; e=edg[e].nxt){
             int y = edg[e].to;
             if(dis[y] > dis[x]+edg[e].d){
                dis[y] = dis[x]+edg[e].d;
                q.push((PPP){y,dis[y]});
             }
          }
       }
       for(int i=1; i<=n; i++){
          printf("%d ",dis[i]);
       }
       return 0;
    }


spfa(个人感觉就是纯bfs)


​    
    #include <cstdio>
    #include <iostream>
    #include <cstring>
    #include <cstdlib>
    #include <algorithm>
    #include <vector>
    #include <map>
    #include <set>
    #include <queue>
    #include <stack>
    typedef long long int LL;
    const int N = 1e4+5, M = 5e5+5,INF = 0x7fffffff;
    int dis[N],head[N];
    bool vis[N];
    int n,m,u,v,d,s;
    int idx;
    struct Edge
    {
       int to,nxt,d;
    }edg[M];
    void addEdge(int fr,int to,int d)
    {
       edg[idx].to = to;
       edg[idx].nxt = head[fr];
       edg[idx].d = d;
       head[fr] = idx++;
    }
    std::queue<int> q;
    int main()
    {
       memset(head,-1,sizeof(head));
       memset(vis,0,sizeof(vis));
       for(int i=0; i<N; i++) dis[i] = INF;
       scanf("%d%d%d",&n,&m,&s);
       for(int i=0; i<m; i++){
          scanf("%d%d%d",&u,&v,&d);
          addEdge(u,v,d);
       }
       dis[s] = 0;
       q.push(s);
       while(!q.empty()){
          int x = q.front(); q.pop(); vis[x] = false;
          for(int e=head[x]; e!=-1; e=edg[e].nxt){
             int y = edg[e].to;
             if(dis[y] > dis[x]+edg[e].d){
                dis[y] = dis[x]+edg[e].d;
                if(!vis[y]){
                   vis[y] = true;
                   q.push(y);
                }
             }
          }
       }
       for(int i=1; i<=n; i++){
          printf("%d ",dis[i]);
       }
       return 0;
    }


## 洛谷P4779 【模板】单源最短路径（标准版）


​    
    #include <cstdio>
    #include <iostream>
    #include <cstring>
    #include <cstdlib>
    #include <algorithm>
    #include <vector>
    #include <map>
    #include <set>
    #include <queue>
    #include <stack>
    typedef long long int LL;
    const int N = 1e5+5, M = 2e5+5,INF = 0x7fffffff;
    int n,m,u,v,d,s;
    int idx;
    int head[N],dis[N];
    bool vis[N];
    struct Edge
    {
       int to,nxt,d;
    }edg[M];
    void addEdge(int fr,int to,int d)
    {
       edg[idx].to = to;
       edg[idx].nxt = head[fr];
       edg[idx].d = d;
       head[fr] = idx++;
    }
    struct PPP
    {
       int val,dis;
       bool operator < (const PPP &x) const{
          return dis > x.dis;
       }
    };
    std::priority_queue<PPP> q;
    int main()
    {
       //freopen("D:\\EdgeDownloadPlace\\P3371_2.in","r",stdin);
       memset(head,-1,sizeof(head));
       for(int i=0;i<N; i++) dis[i] = INF;
       scanf("%d%d%d",&n,&m,&s);
       for(int i=1; i<=m; i++){
          scanf("%d%d%d",&u,&v,&d);
          addEdge(u,v,d);
       }
       dis[s] = 0;
       q.push((PPP){s,0});
       while(!q.empty()){
          PPP cur = q.top(); q.pop();
          int x = cur.val;
          if(vis[x]) continue;
          vis[x] = true;
          for(int e=head[x]; e!=-1; e=edg[e].nxt){
             int y = edg[e].to;
             if(dis[y] > dis[x]+edg[e].d){
                dis[y] = dis[x]+edg[e].d;
                q.push((PPP){y,dis[y]});
             }
          }
       }
       for(int i=1; i<=n; i++){
          printf("%d ",dis[i]);
       }
       return 0;
    }

