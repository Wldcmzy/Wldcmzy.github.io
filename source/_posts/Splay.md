---
title: Splay.md
date: 1111-11-11 11:11:11
categories:
  - [算法, 思想or数据结构]
tags:
  - OldBlog(Before20220505)
  - Splay树
---

## 说明 - 2022-05-05
本篇博客为本人原创, 原发布于CSDN, 在搭建个人博客后使用爬虫批量爬取并挂到个人博客, 出于一些技术原因博客未能完全还原到初始版本(而且我懒得修改), 在观看体验上会有一些瑕疵 ,若有需求会发布重制版总结性新博客。发布时间统一定为1111年11月11日。钦此。

luogu.com.cn P3391 文艺平衡树


​    
```cpp
#include <cstdio>
#include <algorithm>
const int N = 100005;
int n, m, rt, qx, qy;
struct SplayNode{
    int fa, son[2], size, rev, rt;
}snode[N];
inline void pushup(int x){
    int son0 = snode[x].son[0], son1 = snode[x].son[1];
    snode[x].size = snode[son0].size + snode[son1].size + 1;
}
inline void pushdown(int x){
    if(snode[x].rev){
        std::swap(snode[x].son[0], snode[x].son[1]);
        int son0 = snode[x].son[0], son1 = snode[x].son[1];
        snode[son0].rev ^= 1;
        snode[son1].rev ^= 1;
        snode[x].rev = 0;
    }
}
void rotate(int x, int &k){
    int y = snode[x].fa, z = snode[y].fa;
    int y2x = snode[y].son[1] == x, z2y = snode[z].son[1] == y;
    if(y == k) k = x; else snode[z].son[z2y] = x;
    snode[y].son[y2x] = snode[x].son[y2x^1];
    snode[snode[y].son[y2x]].fa = y;
    snode[x].son[y2x^1] = y;
    snode[y].fa = x, snode[x].fa = z;
    pushup(x), pushup(y);
}
void splay(int x, int &k){
    while(x != k){
        int y = snode[x].fa, z = snode[y].fa;
        if(y != k){
            if((snode[y].son[1] == x) ^ (snode[z].son[1] == y)) rotate(x, k);
            else rotate(y, k);
        }
        rotate(x, k);
    }
}
int find(int x, int k){
    pushdown(x);
    int sz = snode[snode[x].son[0]].size;
    if(k == sz + 1) return x;
    if(k <= sz) return find(snode[x].son[0],k);
    else return find(snode[x].son[1], k - sz - 1);
}
void rev(int l, int r){
    int x = find(rt, l), y = find(rt, r);
    splay(x, rt);
    splay(y, snode[x].son[1]);
    int z = snode[y].son[0];
    snode[z].rev ^= 1;
}
void build(int l, int r, int root){
    if(l > r) return ;
    int mid = (l + r) >> 1;
    if(mid < root) snode[root].son[0] = mid;
    else snode[root].son[1] = mid;
    snode[mid].fa = root;
    snode[mid].size = 1;
    if(l == r) return ;
    build(l, mid-1, mid);
    build(mid+1, r, mid);
    pushup(mid);
}
int main(){
    scanf("%d%d",&n,&m);
    rt = (n + 3) >> 1;
    build(1, n+2, rt);
    while(m--){
        scanf("%d%d",&qx,&qy);
        rev(qx, qy+2);
    }
    for(int i=2; i<=n+1; i++) printf("%d ",find(rt, i) - 1);
    return 0;
}
```


luogu.com.cn P3369 普通平衡树


​    
```cpp
#include <cstdio>
#include <iostream>

inline int read(){
    int v = 0, f = 1; char ch = getchar();
    while(!isdigit(ch)){ if(ch == '-') f = -1; ch = getchar(); }
    while(isdigit(ch)){ v = (v << 1) + (v << 3) + ch - 48; ch = getchar(); }
    return v * f;
}

const int N = 1e5+4, HINF = 0x3fffffff;
int fa[N], cnt[N], son[N][2], val[N], size[N];
int root, idx, op, va, n;

inline void update(int x){
    size[x] = size[son[x][0]] + size[son[x][1]] + cnt[x];
}

void rotate(int x){
    int y = fa[x], z = fa[y];
    int y2x = son[y][1] == x, z2y = son[z][1] == y;
    son[z][z2y] = x, fa[x] = z;
    son[y][y2x] = son[x][y2x ^ 1], fa[son[x][y2x ^ 1]] = y;
    son[x][y2x ^ 1] = y , fa[y] = x;
    update(y), update(x);
}

void splay(int x, int goal){
    while(fa[x] != goal){
        int y = fa[x], z = fa[y];
        if(z != goal){
            if((son[z][1] == y) ^ (son[y][1] == x)) rotate(x);
            else rotate(y);
        } 
        rotate(x);
    }
    if(!goal) root = x;
}

void find(int x){
    if(int u = root){
        while(son[u][x > val[u]] && x != val[u]){
            u = son[u][x > val[u]];
        }
        splay(u, 0);
    }
}

void insert(int x){
    int u = root, ffa = 0;
    while(u && val[u] != x){
        ffa = u;
        u = son[u][x > val[u]];
    }
    if(u) cnt[u] += 1;
    else{
        u = ++idx;
        if(ffa) son[ffa][x > val[ffa]] = u;
        son[u][0] = son[u][1] = 0;
        fa[u] = ffa;
        val[u] = x;
        cnt[u] = size[u] = 1;
    }
    splay(u, 0);
}

int pre_next(int x, bool isnext){
    find(x);
    int u = root;
    if(val[u] < x && !isnext) return u;
    if(val[u] > x && isnext) return u;
    u = son[u][isnext];
    while(son[u][isnext ^ 1]) u = son[u][isnext ^ 1];
    return u;
}

void _delete(int x){
    int pre = pre_next(x, 0), next = pre_next(x, 1);
    splay(pre, 0), splay(next, pre);
    int todel = son[next][0];
    if(cnt[todel] > 1){
        cnt[todel] -= 1;
        splay(todel, 0);
    }
    else son[next][0] = 0;
}

int Kth(int x){
    int u = root;
    if(size[u] < x) return 0;
    while(true){
        if(size[son[u][0]] + cnt[u] < x){
            x -= size[son[u][0]] + cnt[u];
            u = son[u][1];
        }
        else{
            if(size[son[u][0]] >= x) u = son[u][0];
            else return val[u];
        }
    }
}

int main(){
    scanf("%d", &n);
    insert(HINF); insert(-HINF);
    while(n--){
        scanf("%d%d", &op, &va);
        if(op == 1) insert(va);
        else if(op == 2) _delete(va);
        else if(op == 3) {
            find(va);
            printf("%d\n", size[son[root][0]]);
        }
        else if(op == 4) printf("%d\n", Kth(va + 1));
        else if(op == 5) printf("%d\n", val[pre_next(va, 0)]);
        else if(op == 6) printf("%d\n", val[pre_next(va, 1)]);
    }
    return 0;
}
```

