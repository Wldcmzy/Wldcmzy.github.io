---
title: 【LCA练习 倍增 tarjin】 洛谷P3379【模板】最近公共祖先 P1967货车运输.md
date: 1111-11-11 11:11:11
categories:
  - [算法, 图论]
tags:
  - LCA
  - OldBlog(Before20220505)
---

## 说明 - 2022-05-05
本篇博客为本人原创, 原发布于CSDN, 在搭建个人博客后使用爬虫批量爬取并挂到个人博客, 出于一些技术原因博客未能完全还原到初始版本(而且我懒得修改), 在观看体验上会有一些瑕疵 ,若有需求会发布重制版总结性新博客。发布时间统一定为1111年11月11日。钦此。

倍增 构造O(nlogn)，在线(单次)查询O(logn)  
tarjin 构造O(n) ，离线(全部)查询O(N+M)  
根据题目 **洛谷P3379【模板】最近公共祖先** 的样例测试，tarjin和倍增不能通过占用的时间或空间评判高下，谁占优势应该跟数据的特征有关。

## 洛谷P3379【模板】最近公共祖先

倍增方法

> 构造fa数组的正确写法是这样的  
>  for(int i=1; i<=20; i++){  
>  fa[x][i] = fa[fa[x][i-1]][i-1];  
>  }  
>  然而第一遍错写成了这样  
>  for(int i=1; i<=20; i++){  
>  fa[x][i] = fa[father][i-1];  
>  }  
>  这显然是错误的，不是方法他的父亲的上2^i-1个节点，而是访问它的上2^i-1个节点的上2^i-1个节点  
>  引以为戒


    #include <iostream>
    #include <cstdio>
    #include <algorithm>
    #include <queue>
    #include <map>
    #include <stack>
    #include <string>
    #include <cstring>
    #include <vector>
    #include <cmath>
    #include <set>
    typedef long long int LL;
    const int N = 5e5+5;
    int fa[N][22],depth[N],head[N];
    int n,m,s,u,v;
    int idx;
    struct Edge
    {
        int to,nxt;
    }edg[N<<1];
    void addEdge(int fr,int to)
    {
        edg[idx].to = to;
        edg[idx].nxt = head[fr];
        head[fr] = idx++;
    }
    void build(int x,int father)
    {
        fa[x][0] = father;
        depth[x] = depth[father]+1;
        for(int i=1; i<=20; i++){
            fa[x][i] = fa[fa[x][i-1]][i-1];
        }
        for(int e=head[x]; e; e=edg[e].nxt){
            int y = edg[e].to;
            if(y!=father) build(y,x);
        }
    }
    int lca(int x,int y)
    {
        if(depth[x] < depth[y]) std::swap(x,y);
        for(int i=20; i>=0; i--){
            if(fa[x][i]) if(depth[fa[x][i]]>=depth[y]){
                x = fa[x][i];
                if(depth[x] == depth[y]) break;
            }
        }
        if(x == y) return x;
        for(int i=20; i>=0; i--){
            if(fa[x][i]) if(fa[x][i] != fa[y][i]){
                x = fa[x][i];
                y = fa[y][i];
            }
        }
        return fa[y][0];
    }
    int main()
    {
        //freopen("D:\\EdgeDownloadPlace\\P3379_2.in","r",stdin);
        idx = 2;
        scanf("%d%d%d",&n,&m,&s);
        for(int i=1; i<n; i++){
            scanf("%d%d",&u,&v);
            addEdge(u,v); addEdge(v,u);
        }
        build(s,0);
        for(int i=0; i<m; i++){
            scanf("%d%d",&u,&v);
            printf("%d\n",lca(u,v));
        }
        return 0;
    }


倍增方法可以提前算log优化


​    
​    #include <iostream>
​    #include <cstdio>
​    #include <algorithm>
​    #include <queue>
​    #include <map>
​    #include <stack>
​    #include <string>
​    #include <cstring>
​    #include <vector>
​    #include <cmath>
​    #include <set>
​    typedef long long int LL;
​    const int N = 5e5+5;
​    int fa[N][22],depth[N],head[N],old_brother[N];
​    int n,m,s,u,v;
​    int idx;
​    struct Edge
​    {
​        int to,nxt;
​    }edg[N<<1];
​    void addEdge(int fr,int to)
​    {
​        edg[idx].to = to;
​        edg[idx].nxt = head[fr];
​        head[fr] = idx++;
​    }
​    void build(int x,int father)
​    {
​        fa[x][0] = father;
​        depth[x] = depth[father]+1;
​    
​        for(int i=1; i<=old_brother[depth[x]]; i++){
​        //for(int i=1; i<=20; i++){
​            fa[x][i] = fa[fa[x][i-1]][i-1];
​        }
​    
        for(int e=head[x]; e; e=edg[e].nxt){
            int y = edg[e].to;
            if(y!=father) build(y,x);
        }
    }
    int lca(int x,int y)
    {
        if(depth[x] < depth[y]) std::swap(x,y);
    //
        /*
        for(int i=20; i>=0; i--){
            if(fa[x][i]) if(depth[fa[x][i]]>=depth[y]){
                x = fa[x][i];
                if(depth[x] == depth[y]) break;
            }
        }
        */
        while(depth[x] > depth[y]){
            x = fa[x][old_brother[depth[x]-depth[y]]-1];
        }
    ///
        if(x == y) return x;
        for(/*int i=20;*/ int i=old_brother[depth[x]]-1; i>=0; i--){
            if(fa[x][i] != fa[y][i]){
                x = fa[x][i];
                y = fa[y][i];
            }
        }
        return fa[y][0];
    }
    int main()
    {
        //freopen("D:\\EdgeDownloadPlace\\P3379_2.in","r",stdin);
        idx = 2;
        scanf("%d%d%d",&n,&m,&s);
        for(int i=1; i<n; i++){
            scanf("%d%d",&u,&v);
            addEdge(u,v); addEdge(v,u);
        }
        for(int i=1; i<=n; i++){
            old_brother[i] = old_brother[i-1] + (1 << old_brother[i-1] == i);
        }
        build(s,0);
        for(int i=0; i<m; i++){
            scanf("%d%d",&u,&v);
            printf("%d\n",lca(u,v));
        }
        return 0;
    }


​    

tarjin方法


​    
​    #include <iostream>
​    #include <cstdio>
​    #include <cstring>
​    #include <string>
​    #include <cstdlib>
​    #include <cmath>
​    #include <algorithm>
​    #include <queue>
​    #include <stack>
​    #include <vector>
​    #include <set>
​    #include <map>
​    typedef long long int LL;
​    //typedef std::pair<int ,int> PP;
​    const int N = 5e5+5, M = 1e6+5, INF = 0x7fffffff;
​    int n, m, u, v, s;
​    int idx,qidx;
​    int head[N],sset[N],vis[N],ans[N],qhead[N];
​    struct Edge
​    {
​        int to, nxt;
​    } edg[M];
​    struct QEdge
​    {
​        int to, nxt, id;
​    } qedg[M];
​    void initial()
​    {
​        idx = qidx = 2;
​        for (int i = 0; i < N; i++)
​            sset[i] = i;
​    }
​    int ffind(int x)
​    {
​        return sset[x] == x ? x : sset[x] = ffind(sset[x]);
​    }
​    void mmerge(int x,int  y)
​    {
​        sset[ffind(x)] = ffind(y);
​    }
​    void addEdge(int fr,int to)
​    {
​        edg[idx].to = to;
​        edg[idx].nxt = head[fr];
​        head[fr] = idx++;
​    }
​    void qaddEdge(int fr,int to,int id)
​    {
​        qedg[qidx].to = to;
​        qedg[qidx].nxt = qhead[fr];
​        qedg[qidx].id = id;
​        qhead[fr] = qidx++;
​    }
​    void tarjin(int x,int fr)
​    {
​        for (int e = head[x]; e; e=edg[e].nxt){
​            if((e^1) == fr)
​                continue;
​            int y = edg[e].to;
​            tarjin(y,e);
​            mmerge(y, x);
​        }
​        for (int e = qhead[x]; e; e=qedg[e].nxt){
​            int y = qedg[e].to;
​            if(vis[y])
​                ans[qedg[e].id] = ffind(y);
​        }
​        vis[x] = 1;
​    }
​    int main()
​    {
​        initial();
​        scanf("%d%d%d",&n,&m,&s);
​        for (int i = 1; i < n; i++){
​            scanf("%d%d",&u,&v);
​            addEdge(u, v);
​            addEdge(v, u);
​        }
​        for (int i = 0; i < m; i++){
​            scanf("%d%d",&u,&v);
​            qaddEdge(u, v, i);
​            qaddEdge(v, u, i);
​        }
​        tarjin(s,0);
​        for (int i = 0; i < m; i++)
​            printf("%d\n",ans[i]);
​        return 0;
​    }


## 洛谷 P1967 [NOIP2013 提高组] 货车运输

最大生成树 + 倍增lca找最短距离  
注意应该先更新ans，再使当前点倍增跳跃，刚才是有一处写反了，没注意到，导致WA


​    
​    #include <iostream>
​    #include <cstdio>
​    #include <cstdlib>
​    #include <cstring>
​    #include <cmath>
​    #include <algorithm>
​    #include <string>
​    #include <queue>
​    #include <vector>
​    #include <stack>
​    #include <map>
​    #include <set>
​    typedef long long int LL;
​    const int N = 1e4+4, M = 5e4+4, INF = 0x7fffffff, LM = 14;
​    int n,m,u,v,w,k,ans;
​    int idx;
​    int head[N],sset[N],fa[N][LM+1],dis[N][LM+1],depth[N],vis[N];
​    struct Esave
​    {
​        int fr,to,w;
​    }es[M<<1];
​    struct Edge
​    {
​        int to,nxt,w;
​    }edg[M<<1];
​    void addE(int u,int v,int w)
​    {
​        edg[idx].to = v;
​        edg[idx].nxt = head[u];
​        edg[idx].w = w;
​        head[u] = idx++;
​    }
​    void initial()
​    {
​        idx = 2;
​        for(int i=0; i<N; i++) sset[i] = i;
​    }
​    bool escmp(Esave x,Esave y)
​    {
​        return x.w > y.w;
​    }
​    int ffind(int x)
​    {
​        return sset[x] == x ? x : sset[x] = ffind(sset[x]);
​    }
​    void mmerge(int x,int y)
​    {
​        sset[ffind(x)] = ffind(y);
​    }
​    void build(int x,int father,int d)
​    {
​        fa[x][0] = father;
​        dis[x][0] = d;
​        depth[x] = depth[father] + 1;
​        for(int i=1; i<=LM; i++){
​            fa[x][i] = fa[fa[x][i-1]][i-1];
​            dis[x][i] = std::min(dis[fa[x][i-1]][i-1],dis[x][i-1]);
​        }
​        for(int e=head[x]; e; e=edg[e].nxt){
​            if(edg[e].to != father) build(edg[e].to,x,edg[e].w);
​        }
​    }
​    int lca(int x,int y)
​    {
​        ans = INF;
​        if(ffind(x) != ffind(y)) return -1;
​        if(depth[x] < depth[y]) std::swap(x,y);
​        for(int i=LM; i>=0; i--){
​            if(fa[x][i]) if(depth[fa[x][i]] >= depth[y]){
​                ans = std::min(ans,dis[x][i]);
​                x = fa[x][i];
​            }
​            if(depth[x] == depth[y]) break;
​        }
​        if(x == y) return ans;
​        for(int i=LM; i>=0; i--){
​            if(fa[x][i]) if(fa[x][i] != fa[y][i]){
​                ans = std::min(ans,dis[x][i]);
​                ans = std::min(ans,dis[y][i]);
​                x = fa[x][i];
​                y = fa[y][i];
​            }
​        }
​        return std::min(ans,std::min(dis[x][0],dis[y][0]));
​        //return ans;
​    }  
​    int main()
​    {
​        initial();
​        scanf("%d%d",&n,&m);
​        for(int i=0; i<m; i++) scanf("%d%d%d",&es[i].fr,&es[i].to,&es[i].w);
​        std::sort(es,es+m,escmp);
​        for(int i=0; i<m;  i++){
​            u = es[i].fr, v = es[i].to, w = es[i].w;
​            if(ffind(u) != ffind(v)){
​                mmerge(u,v);
​                addE(u,v,w); addE(v,u,w);
​            }
​        }
​        for(int i=1; i<=n; i++) if(!vis[ffind(i)]){
​            vis[ffind(i)] = 1;
​            build(i,0,0);
​        }
​        scanf("%d",&k);
​        while(k--){
​            scanf("%d%d",&u,&v);
​            printf("%d\n",lca(u,v));
​        }
​        return 0;
​    }


求log优化版


​    
​    #include <iostream>
​    #include <cstdio>
​    #include <cstdlib>
​    #include <cstring>
​    #include <cmath>
​    #include <algorithm>
​    #include <string>
​    #include <queue>
​    #include <vector>
​    #include <stack>
​    #include <map>
​    #include <set>
​    typedef long long int LL;
​    const int N = 1e4+4, M = 5e4+4, INF = 0x7fffffff, LM = 14;
​    int n,m,u,v,w,k,ans;
​    int idx;
​    int head[N],sset[N],fa[N][LM+1],dis[N][LM+1],depth[N],vis[N],lg[N];
​    struct Esave
​    {
​        int fr,to,w;
​    }es[M<<1];
​    struct Edge
​    {
​        int to,nxt,w;
​    }edg[M<<1];
​    void addE(int u,int v,int w)
​    {
​        edg[idx].to = v;
​        edg[idx].nxt = head[u];
​        edg[idx].w = w;
​        head[u] = idx++;
​    }
​    void initial()
​    {
​        idx = 2;
​        for(int i=0; i<N; i++) sset[i] = i;
​        for(int i=1; i<N; i++) lg[i] = lg[i-1] + (1 << lg[i-1] == i);
​    }
​    bool escmp(Esave x,Esave y)
​    {
​        return x.w > y.w;
​    }
​    int ffind(int x)
​    {
​        return sset[x] == x ? x : sset[x] = ffind(sset[x]);
​    }
​    void mmerge(int x,int y)
​    {
​        sset[ffind(x)] = ffind(y);
​    }
​    void build(int x,int father,int d)
​    {
​        fa[x][0] = father;
​        dis[x][0] = d;
​        depth[x] = depth[father] + 1;
​        for(int i=1; i<=lg[depth[x]]-1; i++){
​            fa[x][i] = fa[fa[x][i-1]][i-1];
​            dis[x][i] = std::min(dis[fa[x][i-1]][i-1],dis[x][i-1]);
​        }
​        for(int e=head[x]; e; e=edg[e].nxt){
​            if(edg[e].to != father) build(edg[e].to,x,edg[e].w);
​        }
​    }
​    int lca(int x,int y)
​    {
​        ans = INF;
​        if(ffind(x) != ffind(y)) return -1;
​        if(depth[x] < depth[y]) std::swap(x,y);
​        /*
​        for(int i=LM; i>=0; i--){
​            if(fa[x][i]) if(depth[fa[x][i]] >= depth[y]){
​                ans = std::min(ans,dis[x][i]);
​                x = fa[x][i];
​            }
​            if(depth[x] == depth[y]) break;
​        }
​        */
​        *
​        while(depth[x] > depth[y]){
​            u = lg[depth[x]-depth[y]]-1;
​            ans = std::min(ans,dis[x][u]);
​            x = fa[x][u];
​        }
​        //*/
​        if(x == y) return ans;
​        for(int i=lg[depth[x]]-1; i>=0; i--){
​            if(fa[x][i]) if(fa[x][i] != fa[y][i]){
​                ans = std::min(ans,dis[x][i]);
​                ans = std::min(ans,dis[y][i]);
​                x = fa[x][i];
​                y = fa[y][i];
​            }
​        }
​        return std::min(ans,std::min(dis[x][0],dis[y][0]));
​        //return ans;
​    }  
​    int main()
​    {
​        initial();
​        scanf("%d%d",&n,&m);
​        for(int i=0; i<m; i++) scanf("%d%d%d",&es[i].fr,&es[i].to,&es[i].w);
​        std::sort(es,es+m,escmp);
​        for(int i=0; i<m;  i++){
​            u = es[i].fr, v = es[i].to, w = es[i].w;
​            if(ffind(u) != ffind(v)){
​                mmerge(u,v);
​                addE(u,v,w); addE(v,u,w);
​            }
​        }
​        for(int i=1; i<=n; i++) if(!vis[ffind(i)]){
​            vis[ffind(i)] = 1;
​            build(i,0,0);
​        }
​        scanf("%d",&k);
​        while(k--){
​            scanf("%d%d",&u,&v);
​            printf("%d\n",lca(u,v));
​        }
​        return 0;
​    }



