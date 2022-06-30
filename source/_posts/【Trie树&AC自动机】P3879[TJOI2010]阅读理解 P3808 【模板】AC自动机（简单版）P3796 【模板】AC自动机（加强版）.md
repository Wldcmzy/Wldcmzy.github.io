---
title: 【Trie树&AC自动机】P3879[TJOI2010]阅读理解 P3808 【模板】AC自动机（简单版）P3796 【模板】AC自动机（加强版）.md
date: 1111-11-11 11:11:11
categories:
  - [算法, 字符串]
tags:
  - OldBlog(Before20220505)
  - AC自动机
  - Trie树
  - 字符串
---

## 说明 - 2022-05-05
本篇博客为本人原创, 原发布于CSDN, 在搭建个人博客后使用爬虫批量爬取并挂到个人博客, 出于一些技术原因博客未能完全还原到初始版本(而且我懒得修改), 在观看体验上会有一些瑕疵 ,若有需求会发布重制版总结性新博客。发布时间统一定为1111年11月11日。钦此。

## 洛谷 P3879[TJOI2010]阅读理解


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
​    const int INF = 0x7fffffff, SCF = 0x3fffffff;
​    const int N = 5e6+4, M = 1e3+4;
​    int n,m,k;
​    int idx,len,ans;
​    int has[M];
​    char ss[22];
​    struct TT
​    {
​        int nxt[26],id;
​    }tree[N];
​    void jointree(int rt)
​    {
​        len = strlen(ss);
​        for(int i=0; i<len; i++){
​            int pp = ss[i]-'a';
​            if(!tree[rt].nxt[pp]){
​                tree[rt].nxt[pp] = idx++;
​            }
​            rt = tree[rt].nxt[pp];
​        }
​        tree[rt].id = 1;
​    }
​    void qry(int rt)
​    {
​        int root = rt;
​        for(int i=0; i<len; i++){
​            int pp = ss[i]-'a';
​            if(!tree[rt].nxt[pp]) return ;
​            rt = tree[rt].nxt[pp];
​        }
​        has[root] = tree[rt].id;
​    }
​    void solve()
​    {
​        memset(has,0,sizeof(has));
​        len = strlen(ss);
​        for(int i=1; i<=n; i++){
​            qry(i);
​        }
​        for(int i=1; i<=n; i++){
​            if(has[i]) printf("%d ",i);
​        }
​        printf("\n");
​    }
​    int main()
​    {
​        scanf("%d",&n);
​        idx = n+1;
​        for(int i=1; i<=n; i++){
​            scanf("%d",&k);
​            for(int j=1; j<=k; j++){
​                scanf("%s",ss);
​                jointree(i);
​            }
​        }
​        scanf("%d",&m);
​        for(int i=0; i<m; i++){
​            scanf("%s",ss);
​            solve();
​        }
​        return 0;
​    }


​    

## 洛谷 P3808 【模板】AC自动机（简单版）


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
​    const int INF = 0x7fffffff, SCF = 0x3fffffff;
​    const int N = 1e7+4, M = 1e3+4;
​    int n,idx;
​    char ss[N];
​    struct TT
​    {
​        int fail,id,nxt[26];
​    }tree[N];
​    void joinTree()
​    {
​        int len = strlen(ss), rt = 0;
​        for(int i=0; i<len; i++){
​            int pp = ss[i]-'a';
​            if(!tree[rt].nxt[pp]){
​                tree[rt].nxt[pp] = idx++;
​            }
​            rt = tree[rt].nxt[pp];
​        }
​        tree[rt].id += 1;
​    }
​    void getFail()
​    {
​        std::queue<int> q;
​        int x,y,fail;
​        for(int i=0; i<26; i++){
​            y = tree[0].nxt[i];
​            if(y>0){
​                tree[y].fail = 0;
​                q.push(y);
​            }
​        }
​        while(!q.empty()){
​            x = q.front(); q.pop();
​            fail = tree[x].fail;
​            for(int i=0; i<26; i++){
​                y = tree[x].nxt[i];
​                if(y>0){
​                    tree[y].fail = tree[fail].nxt[i];
​                    q.push(y);
​                }
​                else tree[x].nxt[i] = tree[fail].nxt[i];
​            }
​        }
​    }
​    int ACgo()
​    {
​        int rt = 0,len = strlen(ss),ans = 0;
​        for(int i=0; i<len; i++){
​            rt = tree[rt].nxt[ss[i]-'a'];
​            for(int j=rt; j && tree[j].id != -1; j=tree[j].fail){
​                ans += tree[j].id;
​                tree[j].id = -1;
​            }
​        }
​        return ans;
​    }
​    int main()
​    {
​        //freopen("D:\\EdgeDownloadPlace\\P3808_2.in","r",stdin);
​        idx = 1;
​        scanf("%d",&n);
​        while(n--){
​            scanf("%s",ss);
​            joinTree();
​        }
​        getFail();
​        scanf("%s",ss);
​        printf("%d",ACgo());
​        return 0;
​    }


## 洛谷 P3796 【模板】AC自动机（加强版）


​    
```cpp
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
const int INF = 0x7fffffff, SCF = 0x3fffffff;
const int N = 1e6+4, M = 10550;
int n;
int idx;
struct TT
{
    int fail,id,nxt[26];
}node[M];
char ss[N];
char sub[155][75];
int save[155];
void initial()
{
    for(int i=0; i<=idx; i++){
        memset(node[i].nxt,0,sizeof(node[i].nxt));
        node[i].fail = 0;
        node[i].id = 0;
    }
    idx = 0;
    memset(save,0,sizeof(save));
}
void joinTree(int id)
{
    int len = strlen(sub[id]) ,rt = 0;
    for(int i=0; i<len; i++){
        int ch = sub[id][i]-'a';
        if(!node[rt].nxt[ch]) node[rt].nxt[ch] = ++idx;
        rt = node[rt].nxt[ch];
    }
    node[rt].id = id;
}
void buildFail()
{
    std::queue<int >q;
    for(int i=0; i<26; i++){
        if(node[0].nxt[i]>0) {
            node[node[0].nxt[i]].fail = 0;
            q.push(node[0].nxt[i]);
        }
    }
    while(!q.empty()){
        int cur = q.front(); q.pop();
        int fail = node[cur].fail;
        for(int i=0; i<26; i++){
            int next = node[cur].nxt[i];
            if(next>0){
                node[next].fail = node[fail].nxt[i];
                q.push(next);
            }
            else node[cur].nxt[i] = node[fail].nxt[i];
        }
    }
}
void ACgo()
{
    int len = strlen(ss),rt = 0;
    for(int i=0; i<len; i++){
        rt = node[rt].nxt[ss[i]-'a'];
        for(int j=rt; j; j=node[j].fail)
            save[node[j].id] += 1;
    }
}
int main()
{
    //freopen("D:\\EdgeDownloadPlace\\P3796_1.in","r",stdin);
    while(scanf("%d",&n)){
        if(n == 0) break;
        initial();
        for(int i=1; i<=n; i++){
            scanf("%s",sub[i]);
            joinTree(i);
        }
        buildFail();
        scanf("%s",ss);
        ACgo();
        int mx = 0;
        for(int i=1; i<=n; i++){
            mx = std::max(save[i],mx);
        }
        printf("%d\n",mx);
        for(int i=1; i<=n; i++){
            if(save[i] == mx){
                printf("%s\n",sub[i]);
            }
        }
    }
    return 0;
}
```


​    

