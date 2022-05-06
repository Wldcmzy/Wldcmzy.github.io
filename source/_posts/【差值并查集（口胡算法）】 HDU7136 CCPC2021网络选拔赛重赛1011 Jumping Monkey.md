---
title: 【差值并查集（口胡算法）】 HDU7136 CCPC2021网络选拔赛重赛1011 Jumping Monkey.md
date: 1111-11-11 11:11:11
categories:
  - [算法, 图论]
tags:
  - OldBlog(Before20220505)
  - 并查集
---

## 说明 - 2022-05-05
本篇博客为本人原创, 原发布于CSDN, 在搭建个人博客后使用爬虫批量爬取并挂到个人博客, 出于一些技术原因博客未能完全还原到初始版本(而且我懒得修改), 在观看体验上会有一些瑕疵 ,若有需求会发布重制版总结性新博客。发布时间统一定为1111年11月11日。钦此。

## 题面

HDU7136 不放了

## 思路

把所有的点按照权值从小到大排序， 依次用并查集维护  
每次更新一个点， 把此点与其相邻的枚举过的点并为一个集合， 这样每次更新可以使这个集合的点的遍历数量都+1  
用num[]记录一个点可以合法遍历的点的数量， 且num[]仅对代表元生效  
用gap[]记录一个点于其代表元的差值, 非代表元的答案为 num[] - gap[] (因为代表元的gap[] = 0)  
记得路径压缩， 不路径压缩——反正我没试过， 会T吧

## 变量&数组 解释

###### 建图存图用的 ：

head[], idx, u, v, edg[]

###### 输入数据相关

input[] 输入数据  
node[] 输入数据 结构体 {输入数值 + 编号} 后按照输入数值排序

###### 并查集相关

sset[] 并查集  
gap[]每个元素与其代表元素的差值  
num[]从该元素开始可以合法遍历的点的数量  
_**num[]只对代表元生效， 所有节点可合法遍历的点的数量 都可以由 num[] - gap[]计算**_

###### 其他

boss 在每加入一个点时， 作为这一轮的唯一代表元

## 代码

时间826MS 空间5688K


​    
    #include <bits/stdc++.h>
    const int N = 1e5 + 5;
    int t, n, u, v, idx, boss;
    int head[N], sset[N], gap[N], num[N], inpt[N];
    struct AAAA{ 
        int id, w;
        bool operator < (const AAAA &a) const{
            return w < a.w;
        } 
    } node[N];
    struct Edge{ int to, nxt; }edg[N << 1];
    inline void addE(int fr, int to){
        edg[++ idx] = (Edge) {to, head[fr]};
        head[fr] = idx;
    }
    int ffind(int x){
        if(sset[x] == x) return x;
        int fa = sset[x];
        int xx = ffind(fa);
        gap[x] += gap[fa];
        sset[x] = xx;
        return xx;
    }
    inline void mmerge(int fa, int x){
        int ff = ffind(fa), xx = ffind(x);
        sset[xx] = ff;
        gap[xx] = num[ff] - num[xx];
    }
    int main(){
        scanf("%d", &t);
        while(t --){
            idx = 1;
            scanf("%d", &n);
            for(int i=0; i<=n; i++) sset[i] = i;
            for(int i=1; i<n; i++){
                scanf("%d%d", &u, &v);
                addE(u, v), addE(v, u);
            }
            for(int i=1; i<=n; i++){
                scanf("%d", &inpt[i]);
                node[i].w = inpt[i];
                node[i].id = i;
            }
            std::sort(node + 1, node + 1 + n);
            for(int i=1, x, y; i<=n; i++){
                boss = x = node[i].id;
                for(int e=head[x]; e; e=edg[e].nxt){
                    y = edg[e].to;
                    if(inpt[y] < inpt[x]){
                        if(boss == x){
                            boss = y;
                            mmerge(boss, x);
                        }
                        else{
                            mmerge(boss, y);
                        }
                    }
                }
                num[ffind(boss)] += 1;
            }
            for(int x=1, xx; x<=n; x++){
                xx = ffind(x);
                printf("%d\n", num[xx] - gap[x]);
            }
            memset(num, 0, sizeof(int) * (n + 1));
            memset(gap, 0, sizeof(int) * (n + 1));
            memset(head, 0, sizeof(int) * (n + 1));
        }
        return 0;
    }

