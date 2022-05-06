---
title: LCT
date: 1111-11-11 11:11:11
categories:
  -	[算法, 思想or数据结构]
tags:
  - OldBlog(Before20220505)
  - Splay树
  - LCT
---

## 说明 - 2022-05-05

本篇博客为本人原创, 原发布于CSDN, 在搭建个人博客后使用爬虫批量爬取并挂到个人博客, 出于一些技术原因博客未能完全还原到初始版本(而且我懒得修改), 在观看体验上会有一些瑕疵 ,若有需求会发布重制版总结性新博客。发布时间统一定为1111年11月11日。钦此。

## P4219 [BJOI2014]大融合

```cpp
#include <cstdio>
#include <algorithm>
int n, m, op, a, b, x , y;
const int N = 3e5+4;
int top, sdk[N], son[N][2], fa[N],rev[N], sxor[N], sxor2[N];
inline void pushup(int x){
    sxor[x] = sxor[son[x][0]] + sxor[son[x][1]] + sxor2[x] + 1;
}
inline void hpushdown(int x){
    if(rev[x]){
        rev[son[x][0]] ^= 1;
        rev[son[x][1]] ^= 1;
        rev[x] ^= 1;
        std::swap(son[x][0], son[x][1]);
    }
}
inline bool isroot(int x){
    return son[fa[x]][0] != x && son[fa[x]][1] != x;
}
inline void pushdown(int x){
    if(!isroot(x)) pushdown(fa[x]);
    hpushdown(x);
}
void rotate(int x){
    int y = fa[x], z = fa[y];
    int z2y = son[z][1] == y, y2x = son[y][1] == x;
    if(!isroot(y)) son[z][z2y] = x;
    fa[x] = z, fa[y] = x, fa[son[x][y2x ^ 1]] = y;
    son[y][y2x] = son[x][y2x ^ 1], son[x][y2x ^ 1] = y;
    pushup(y); pushup(x);
}
void splay(int x){
    top = 0; sdk[++top] = x;
    for(int i=x; !isroot(i); i = fa[i]) sdk[++top] = fa[i];
    for(int i=top; i; i--) pushdown(sdk[i]);
    while(!isroot(x)){
        int y = fa[x], z = fa[y];
        if(!isroot(y)){
            if((son[y][0] == x) ^ (son[z][0] == y)) rotate(x);
            else rotate(y);
        }
        rotate(x);
    }
}
void access(int x){
    for(int t = 0; x; t = x, x = fa[x]){
        splay(x);
        sxor2[x] += sxor[son[x][1]] - sxor[t];
        son[x][1] = t;
        pushup(x);
    }
}
void makeroot(int x){
    access(x); splay(x); rev[x] ^= 1;
}
int find(int x){
    access(x); splay(x);
    while(son[x][0]) x = son[x][0];
    return x;
}
void split(int x, int y){
    makeroot(x); access(y); splay(y);
}
void cut(int x, int y){
    split(x, y);
    if(son[y][0] == x && son[x][1] == 0){
        son[y][0] = 0; 
        fa[x] = 0;
    }
}
void link(int x, int y){
    makeroot(x);
    fa[x] = y;
}
int main(){
    scanf("%d%d",&n,&m);
    while(m--){
        scanf("%*c%c%d%d",&op,&x,&y);
        if(op=='A')
        {
            makeroot(x);
            makeroot(y);
            fa[x]=y;
            sxor2[y]+=sxor[x];
        }
        if(op=='Q')
        {
            makeroot(x); access(y); splay(y);
            son[y][0] = fa[x] = 0;
            pushup(x);
            makeroot(x); 
            makeroot(y);
            printf("%lld\n",(long long)(sxor[x]*sxor[y]));
            makeroot(x);
            makeroot(y);
            fa[x]=y;
            sxor2[y]+=sxor[x];
        }
    }

    return 0;
}
```

