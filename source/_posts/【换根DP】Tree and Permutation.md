---
title: 【换根DP】Tree and Permutation.md
date: 1111-11-11 11:11:11
categories:
  - [算法, DP]
tags:
  - OldBlog(Before20220505)
  - DP
---

## 说明 - 2022-05-05
本篇博客为本人原创, 原发布于CSDN, 在搭建个人博客后使用爬虫批量爬取并挂到个人博客, 出于一些技术原因博客未能完全还原到初始版本(而且我懒得修改), 在观看体验上会有一些瑕疵 ,若有需求会发布重制版总结性新博客。发布时间统一定为1111年11月11日。钦此。

# Tree and Permutation HUD6446

## 题目大意

给你一颗n个点的树， 有边权，求这个东西：  
Σ i = 1 n Σ j = 1 n d i s [ i t o j ] ∗ ( n − 1 ) ! \Sigma_{i = 1} ^n
\Sigma_{j = 1}^n dis[i~to~j] * (n - 1) ! Σi=1n​Σj=1n​dis[i to j]∗(n−1)!

## 换根dp

用换根dp算前面一坨， 然后 * (n - 1)!  
先dfs（类似树形dp）求任意一个点a到所有点的距离part[a]。  
然后可以由已知点x的答案part[x]做简单变换， 推导出与x相邻的未知答案的点集Y (向量)的答案part[Y] (向量)， 所以又是一次DFS  
然后对所有part求Sigma， 再乘 (n-1)! 记得取模。 完毕。

## 代码


​    
    #include <bits/stdc++.h>
    const int N = 1e5 + 4, MOD = 1e9 + 7;
    int n, u, v, ipart = 1, idx;
    int size[N], head[N];
    long long dis[N], part[N], re, w;
    struct Edge{ int to, nxt; long long w; }edg[N << 1];
    inline void addE(int fr, int to, long long w){
    	edg[++ idx] = (Edge){to, head[fr], w}; head[fr] = idx;
    	edg[++ idx] = (Edge){fr, head[to], w}; head[to] = idx;
    }
    void dfs(int x, int fa){
    	size[x] = 1;
    	for(int e=head[x]; e; e=edg[e].nxt){
    		int y = edg[e].to;
    		if(y == fa) continue;
    		dis[y] = dis[x] + edg[e].w;
    		dfs(y, x);
    		size[x] += size[y];
    	}
    	part[ipart] = (part[ipart] + dis[x]) % MOD;
    }
    void dfsN(int x, int fa){
    	re = (re + part[x]) % MOD;
    	for(int e=head[x]; e; e=edg[e].nxt){
    		int y = edg[e].to;
    		if(y == fa) continue;
    		part[y] = part[x] + (n - (size[y] << 1)) * edg[e].w;
    		dfsN(y, x);
    	}
    }
    int main(){
    	while(scanf("%d", &n) != EOF){
    		idx = 1, re = 0, part[ipart] = 0;
    		memset(head, 0, sizeof(head));
    		memset(size, 0, sizeof(size));
    		memset(dis, 0, sizeof(dis));
    		for(int i=1; i<n; i++){
    			scanf("%d%d%d", &u, &v, &w);
    			addE(u, v, w);
    		}
    		dfs(ipart, -1);
    		dfsN(ipart, -1);
    		re = (re + MOD) % MOD;
    		for(int i=n-1; i>=1; i--) re = re * i % MOD;
    		printf("%lld\n", re);
    	}
    	return 0;
    }


## 描述

There are N vertices connected by N−1 edges, each edge has its own length.  
The set { 1,2,3,…,N } contains a total of N! unique permutations, let’s say
the i-th permutation is Pi and Pi,j is its j-th number.  
For the i-th permutation, it can be a traverse sequence of the tree with N
vertices, which means we can go from the Pi,1-th vertex to the Pi,2-th vertex
by the shortest path, then go to the Pi,3-th vertex ( also by the shortest
path ) , and so on. Finally we’ll reach the Pi,N-th vertex, let’s define the
total distance of this route as D(Pi) , so please calculate the sum of D(Pi)
for all N! permutations.

## Input

There are 10 test cases at most.  
The first line of each test case contains one integer N ( 1≤N≤105 ) .  
For the next N−1 lines, each line contains three integer X, Y and L, which
means there is an edge between X-th vertex and Y-th of length L (
1≤X,Y≤N,1≤L≤109 ) .

## Output

For each test case, print the answer module 109+7 in one line.

