---
title: 【树的直径】树形DP求解.md
date: 1111-11-11 11:11:11
categories:
  - [算法, 图论]
tags:
  - OldBlog(Before20220505)
  - DP
---

## 说明 - 2022-05-05
本篇博客为本人原创, 原发布于CSDN, 在搭建个人博客后使用爬虫批量爬取并挂到个人博客, 出于一些技术原因博客未能完全还原到初始版本(而且我懒得修改), 在观看体验上会有一些瑕疵 ,若有需求会发布重制版总结性新博客。发布时间统一定为1111年11月11日。钦此。

## 思路

每次记录每个子树从根到叶的最长的链， 然后把最长前2长的拼起来  
先更新D再更新dp， 否则会求出一条最长链*2


​    
    void dfs(int x, int efr){
        for(int e = head[x]; e; e = edg[e].nxt){
            if((e ^ 1) == efr) continue;
            int y = edg[e].to;
            dfs(y, e);
            D = std::max(D, dp[x] + dp[y] + 1);
            dp[x] = std::max(dp[x], dp[y] + 1);
        }
    }


## HDU 6201 transaction transaction transaction（非模题）


​    
    #include <bits/stdc++.h>
    const int N = 1e5 + 4;
    int val[N], dp[N][2], head[N];
    struct Edge{
        int to, nxt, va;
    }edg[N << 1];
    int idx, n, t, u, v, w, ans;
    void addE(int fr, int to , int w){
        edg[idx].to = to;
        edg[idx].nxt = head[fr];
        edg[idx].va = w;
        head[fr] = idx ++;
    }
    void dfs(int x, int efr){
        dp[x][0] = val[x], dp[x][1] = -val[x];
        for(int e=head[x]; e; e=edg[e].nxt){
            if((e ^ 1) == efr) continue;
            int y = edg[e].to;
            dfs(y, e);
            dp[x][0] = std::min(dp[x][0], dp[y][0] + edg[e].va);
            dp[x][1] = std::min(dp[x][1], dp[y][1] + edg[e].va);
        }
        ans = std::min(ans, dp[x][0] + dp[x][1]);
    }
    int main(){
        scanf("%d", &t);
        while(t --){
            scanf("%d", &n);
            idx = 2, ans = 0;
            for(int i=0; i<=n; i++) head[i] = 0;
            for(int i=1; i<=n; i++) scanf("%d", &val[i]);
            for(int i=1; i<n; i++){
                scanf("%d%d%d", &u, &v, &w);
                addE(u, v, w), addE(v, u, w);
            }
            dfs(1, -1);
            printf("%d\n", -ans);
        }
        return 0;
    }

