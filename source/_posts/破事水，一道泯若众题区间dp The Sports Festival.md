---
title: 破事水，一道泯若众题区间dp The Sports Festival.md
date: 1111-11-11 11:11:11
categories:
  - [算法, DP]
tags:
  - OldBlog(Before20220505)
  -	DP
---

## 说明 - 2022-05-05
本篇博客为本人原创, 原发布于CSDN, 在搭建个人博客后使用爬虫批量爬取并挂到个人博客, 出于一些技术原因博客未能完全还原到初始版本(而且我懒得修改), 在观看体验上会有一些瑕疵 ,若有需求会发布重制版总结性新博客。发布时间统一定为1111年11月11日。钦此。

## The Sports Festival

#### 题目描述

给 你 一 个 序 列 ( 数 据 可 重 复 ) ， 你 可 以 将 其 任 意 排 序 ， 最 小 化 Σ i = 1 n ( 前 i 个 数 的 极
差 ) 给你一个序列(数据可重复)， 你可以将其任意排序， 最小化 \\\ \Sigma_{i = 1} ^ n (前i个数的极差)
给你一个序列(数据可重复)，你可以将其任意排序，最小化Σi=1n​(前i个数的极差)

#### 思路

0.排一下序  
1.首先我们不可能先选一个最大的，再选一个最小的， 如果这样选就得到了最大值， 就g了， 除非数据特殊  
2.那我们肯定是要循序渐进一点点增大极差， 那就必须要选定某一个点开始， 然后慢慢把范围往两边阔  
3.那我们直接一波区间dp求出所有的情况并选个最小的就行了


​    
```cpp
#include <bits/stdc++.h>
#define int long long
const int N = 2005;
int dp[N][N], x[N], n;
signed main(){
    scanf("%lld", &n);
    for(int i = 1; i <= n; i ++) scanf("%lld", &x[i]);
    std::sort(x + 1, x + 1 + n);
    for(int s = 1; s < n; s ++) for(int st = 1, ed; (ed = st + s) <= n; st ++){
        dp[st][ed] = std::min(dp[st + 1][ed], dp[st][ed - 1]) + x[ed] - x[st];
    }
    printf("%lld", dp[1][n]);
    return 0;
}
```

