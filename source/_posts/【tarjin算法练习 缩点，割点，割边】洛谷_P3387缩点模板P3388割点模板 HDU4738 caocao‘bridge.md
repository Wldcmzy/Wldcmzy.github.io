---
title: 【tarjin算法练习 缩点，割点，割边】洛谷_P3387缩点模板P3388割点模板 HDU4738 caocao‘bridge.md
date: 1111-11-11 11:11:11
categories:
  - OldBlog(Before20220505)
tags:
  - OldBlog(Before20220505)
---

## 说明 - 2022-05-05
本篇博客为本人原创, 原发布于CSDN, 在搭建个人博客后使用爬虫批量爬取并挂到个人博客, 出于一些技术原因博客未能完全还原到初始版本(而且我懒得修改), 在观看体验上会有一些瑕疵 ,若有需求会发布重制版总结性新博客。发布时间统一定为1111年11月11日。钦此。

## 洛谷 P3387缩点模板

vis数组是必要的，如果遇到vis[i]=false的点，说明此点对应的强连通已经被缩点过了,而访问的i的起点并不属于这一强连通分量。若这时执行low[x]
= std::min(low[x],dfn[y]);可能会导致一些点在栈内无法弹出。

    
    
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
    //typedef std::pair<int ,int> PP;
    const int N = 1e4+5, M = 1e5+5;
    int vis[N],low[N],dfn[N],val[N],head[N],stk[N],suo[N],weight[N],hh[N],in[N],dp[N];
    struct Edge
    {
       int to,nxt;
    }edg[M<<1];
    int n,m,u,v;
    int idx,timer,top,id,tt;
    void addEdge(int fr,int to)
    {
       edg[idx].to  = to;
       edg[idx].nxt = head[fr];
       head[fr] = idx++;
    }
    void tarjin(int x)
    {
       low[x] = dfn[x] = ++timer;
       stk[++top] = x,vis[x] = 1;
       for(int e=head[x]; e!=-1; e=edg[e].nxt){
          int y = edg[e].to;
          if(!dfn[y]){
             tarjin(y);
             low[x] = std::min(low[x],low[y]);
          }
          else if(vis[y]){
             low[x] = std::min(low[x],dfn[y]);
          }
       }
       if(low[x] == dfn[x]){
          id += 1;
          while(true){
             vis[stk[top]] = 0;
             suo[stk[top]] = id;
             weight[id] += val[stk[top]];
             if(x == stk[top--]) break;
          }
       }
    }
    void rebuild()
    {
       for(int i=1; i<=n; i++){
          for(int e=head[i]; e!=-1; e=edg[e].nxt){
             int y  = edg[e].to;
             int ii = suo[i],yy = suo[y];
             if(ii != yy){
                in[yy] += 1;
                edg[idx].to = yy;
                edg[idx].nxt = hh[ii];
                hh[ii] = idx++;
             }
          }
       }
    }
    int tpsort()
    {
       std::queue<int> q;
       for(int i=1; i<=id; i++){
          if(!in[i]){
             q.push(i);
             dp[i] = weight[i];
          }
       }
       while(!q.empty()){
          int cur = q.front(); q.pop();
          for(int e=hh[cur]; e!=-1; e=edg[e].nxt){
             int  to = edg[e].to;
             dp[to] = std::max(dp[to],dp[cur]+weight[to]);
             in[to] -= 1;
             if(!in[to]) q.push(to);
          }
       }
       int ans = 0;
       for(int i=1; i<=id; i++){
          ans = std::max(ans,dp[i]);
       }
       return ans;
    }
    int main()
    {
       memset(head,-1,sizeof(head));
       memset(hh,-1,sizeof(hh));
       scanf("%d%d",&n,&m);
       for(int i=1; i<=n; i++){
          scanf("%d",&val[i]);
       }
       for(int i=0; i<m; i++){
          scanf("%d%d",&u,&v);
          addEdge(u,v);
       }
       for(int i=1; i<=n; i++){
          if(!dfn[i]) tarjin(i);
       }
       rebuild();
       printf("%d",tpsort());
    
       return 0;
    }
    

## 洛谷P3388割点模板

trajin中的形参意义为（此点，上一个点），最好改为（此点，过来的边），在做caocao‘s bridge的时候暴露了一些问题

    
    
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
    const int N = 2e4+5, M = 2e5+5;
    int n,m,u,v;
    int ans;
    int idx,timer;
    int head[N];
    int low[N],dfn[N],flag[N];
    struct Edge
    {
       int to,nxt;
    }edg[M];
    void addEdge(int fr,int to)
    {
       edg[idx].to = to;
       edg[idx].nxt = head[fr];
       head[fr] = idx++;
    }
    void tarjin(int x,int fa)
    {
       low[x] = dfn[x] = ++timer;
       int son = 0;
       for(int e=head[x]; e!=-1; e=edg[e].nxt){
          int to = edg[e].to;
          if(!dfn[to]){
             son += 1;
             tarjin(to,x);
             low[x] = std::min(low[x],low[to]);
             if(fa && dfn[x] <= low[to] && !flag[x]){
                flag[x] = 1;
                ans += 1;
             }
          }
          else if(to != fa)low[x] = std::min(low[x],dfn[to]);
       }
       if(!fa && son>=2 && !flag[x]){
          flag[x] = 1;
          ans += 1;
       }
    }
    int main()
    {
       //freopen("D:\\EdgeDownloadPlace\\P3388_4.in","r",stdin);
       memset(head,-1,sizeof(head));
       scanf("%d%d",&n,&m);
       for(int i=0; i<m; i++){
          scanf("%d%d",&u,&v);
          addEdge(u,v); addEdge(v,u);
       }
       for(int i=1; i<=n; i++){
         tarjin(i,0);
       }
       printf("%d\n",ans);
       for(int i=1; i<=n; i++){`在这里插入代码片`
          if(flag[i]) printf("%d ",i);
       }
       return 0;
    }
    

## HDU4738 caocao’bridge

坑点：

  1. 你需要判断图是否是全部联通的，若不是输出0
  2. 若需要派出0个人，则要输出1，至少需要1人才能炸桥

——————  
最开始在tarjin记录（此点，上一个点），一直wa，猜测可能是有重边，改为记录（此点，过来的边）ac。懒得动脑以后再想。

    
    
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
    const int N = 1005, M = 1000005,INF = 1e6;
    int n,m,u,v,p;
    int ans;
    int head[N],low[N],dfn[N],pp[M];
    int idx,timer;
    struct Edge
    {
       int to,nxt,p;
    }edg[M];
    void addEdge(int u,int v,int p)
    {
       edg[idx].to = v;
       edg[idx].nxt = head[u];
       edg[idx].p = p;
       head[u] = idx++;
    }
    void initial()
    {
       idx = timer = 0;
       ans = INF;
       memset(head,-1,sizeof(head));
       memset(low,0,sizeof(low));
       memset(dfn,0,sizeof(dfn));
       memset(pp,0,sizeof(pp));
    }
    void tarjin(int x,int bri)
    {
       low[x] = dfn[x] = ++timer;
       for(int e=head[x]; e!=-1; e=edg[e].nxt){
          if((e^1) == bri) continue;
          int y = edg[e].to;
          if(!dfn[y]){
             tarjin(y,e);
             low[x] = std::min(low[x],low[y]);
             if(dfn[x] < low[y]) pp[e] = pp[e^1] = 1;
          }
          else low[x] = std::min(low[x],dfn[y]);
       }
    }
    int main()
    {
       while(scanf("%d%d",&n,&m)){
          if(n == 0 && m == 0) break;
          initial();
          for(int i=0; i<m; i++){
             scanf("%d%d%d",&u,&v,&p);
             addEdge(u,v,p); addEdge(v,u,p);
          }
          tarjin(1,-1);
          bool judge = false;
          for(int i=1; i<=n; i++){
             if(!dfn[i]){
                judge = true;
                break;
             }
          }
          if(judge) printf("0\n");
          else{
             for(int e=0; e<idx; e++){
                if(pp[e] == 1) ans = std::min(ans,edg[e].p);
             }
             if(!ans) ans += 1;
             else if(ans == INF) ans = -1;
             printf("%d\n",ans);
          }
       }
       return 0;
    }
    
    

## 洛谷P2341 [USACO03FALL][HAOI2006]受欢迎的牛 G

    
    
    #include <iostream>
    #include <cstdio>
    #include <cstring>
    #include <string>
    #include <cstdlib>
    #include <cmath>
    #include <algorithm>
    #include <queue>
    #include <stack>
    #include <vector>
    #include <set>
    #include <map>
    typedef long long int LL;
    typedef std::pair<int ,int> PP;
    const int N = 1e4, M = 5e4+5, INF = 0x7fffffff;
    int n,m,u,v,su,sv,cnt;
    int idx,timer,counter;
    int head[N],low[N],dfn[N],vis[N],suo[N],num[N],out[N];
    PP qry[M];
    std::stack<int> stk;
    struct Edge
    {
        int to,nxt;
    }edg[M];
    void initial()
    {
        idx = 2;
    }
    void addEdge(int fr,int to)
    {
        edg[idx].to =to;
        edg[idx].nxt = head[fr];
        head[fr] = idx++;
    }
    void tarjin(int x)
    {
        dfn[x] = low[x] = ++timer;
        vis[x] = true;
        stk.push(x);
        for(int e=head[x]; e; e=edg[e].nxt){
            int y = edg[e].to;
            if(!dfn[y]){
                tarjin(y);
                low[x] = std::min(low[x],low[y]);
            }
            else if(vis[y]) low[x] = std::min(low[x],dfn[y]);
        }
        if(low[x] == dfn[x]){
            counter += 1;
            while(true){
                int t = stk.top(); stk.pop();
                suo[t] = counter;
                num[counter] += 1;
                vis[t] = false;
                if(t == x) break;
            }
        }
    }
    int main()
    {
        initial();
        scanf("%d%d",&n,&m);
        for(int i=0; i<m; i++){
            scanf("%d%d",&qry[i].first,&qry[i].second);
            addEdge(qry[i].first,qry[i].second);
        }
        for(int i=1; i<=n; i++) if(!dfn[i]) tarjin(i);
        for(int i=0; i<m; i++){
            u = qry[i].first , v = qry[i].second;
            su = suo[u] , sv = suo[v];
            if(su == sv) continue;
            out[su] += 1;
        }
        for(int i=1; i<=counter; i++){
            if(!out[i]){
                cnt += 1;
                u = i;
                if(cnt > 1) break;
            }
        }
        if(cnt == 1) printf("%d",num[u]);
        else printf("0");
        return 0;
    }
    

