---
title: 【数位DP】 洛谷 P2602 [ZJOI2010]数字计数 P2657 [SCOI2009] windy 数.md
date: 1111-11-11 11:11:11
categories:
  - [算法, DP]
tags:
  - OldBlog(Before20220505)	
  - DP
---

## 说明 - 2022-05-05
本篇博客为本人原创, 原发布于CSDN, 在搭建个人博客后使用爬虫批量爬取并挂到个人博客, 出于一些技术原因博客未能完全还原到初始版本(而且我懒得修改), 在观看体验上会有一些瑕疵 ,若有需求会发布重制版总结性新博客。发布时间统一定为1111年11月11日。钦此。

## 洛谷 P2602 [ZJOI2010]数字计数


​    
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
    const int N = 14, M = 1e4+5, K = 1e6+4, T = 10;
    LL dp[N][T][T];
    int dig[N];
    LL a,b;
    LL Qmul(int p)
    {
        LL base = 10, re = 1;
        while(p){
            if(p&1) re *= base;
            base *= base;
            p >>= 1;
        }
        return re;
    }
    LL fun(LL x,int num)
    {
        LL ans = 0;
        int len = 0;
        while(x){
            dig[++len] = x%10;
            x /= 10;
        }
        for(int i=1; i<len; i++){
            for(int j=1; j<T; j++){
                ans += dp[i][j][num];
            }
        }
        for(int i=1; i<dig[len]; i++){
            ans += dp[len][i][num];
        }
        for(int i=1; i<len; i++){
            for(int j=0; j<dig[i]; j++){
                ans += dp[i][j][num];
            }
            for(int j=i+1; j<=len; j++){
                if(dig[j] == num) ans += dig[i]*Qmul(i-1);
                //要算上前面的数
            }
        }
        return ans;
    }
    int main()
    {
        scanf("%lld%lld",&a,&b);
        for(int i=0; i<T; i++) dp[1][i][i] = 1;
        for(int i=2; i<=13; i++){
            for(int j=0; j<T; j++){
                for(int k=0; k<T; k++){
                    for(int r=0; r<T; r++){
                        dp[i][j][r] += dp[i-1][k][r];
                    }
                }
                dp[i][j][j] += Qmul(i-1);
            }
        }
        for(int i=0; i<T; i++) printf("%lld ",fun(b+1,i)-fun(a,i));
        return 0;
    }


## 洛谷 P2657 [SCOI2009] windy 数

抄完数字计数之后照葫芦画瓢做的这道题，遂去看了题解。  
离AC只差一句题解上抄来的 if(std::abs(dig[i+1]-dig[i]) <= 1)
break;，因为函数的参数可能就是一个非windy数，你需要找出这个比这个非windy数小的所有的的windy数，当if(std::abs(dig[i+1]-dig[i])
<= 1) break;这个条件满足时，之后找到的就必然时非windy数了，自然不需要继续找。  
~~之前我自己的类似的判断思路错了，写题解的人yyds~~


​    
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
    const int N = 12, M = 1e4+5, T = 10;
    LL dp[N][N];
    int dig[N];
    int a,b;
    void build()
    {
        for(int i=0; i<T; i++) dp[1][i] = 1;
        for(int i=2; i<=T; i++) {
            for(int j=0; j<T; j++){
                for(int k=0; k<T; k++){
                    if(std::abs(j-k) <= 1) continue;
                    dp[i][j] += dp[i-1][k];
                }
            }
        }
    }
    LL fun(int x)
    {
        LL ans = 0;
        int len = 0;
        while(x){
            dig[++len] = x%10;
            x /= 10;
        }
        for(int i=1; i<len; i++){
            for(int j=1; j<T; j++){
                ans += dp[i][j];
            }
        }
        //std::cout << " pre: " << ans;
        for(int j=1; j<dig[len]; j++){
            ans += dp[len][j];
        }
        //std::cout << " now:" << ans;
        //if(len >=2 && std::abs(dig[len]-dig[len-1]) <= 1) return ans;
        for(int i=len-1; i>=1; i--){
            for(int j=0; j<dig[i]; j++){
                if(std::abs(j-dig[i+1]) <= 1) continue;
                ans += dp[i][j];
            }
            if(std::abs(dig[i+1]-dig[i]) <= 1) break;
        }
        //std::cout << " post:" << ans << " ";
        return ans;
    }
    int main()
    {
        scanf("%d%d",&a,&b);
        build();
        /*
        for(int i=1; i<141; i++){
            std::cout << "fun(" << i << "): " << fun(i+1) << std::endl;
        }
        */
        printf("%lld",fun(b+1)-fun(a));
        return 0;
    }

