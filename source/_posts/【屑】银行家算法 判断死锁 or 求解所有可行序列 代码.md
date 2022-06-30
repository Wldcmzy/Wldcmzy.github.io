---
title: 【屑】银行家算法 判断死锁 or 求解所有可行序列 代码.md
date: 1111-11-11 11:11:11
categories:
  - [教练我想学挂边躲牛, 杂乱]
tags:
  - OldBlog(Before20220505)
---

## 说明 - 2022-05-05
本篇博客为本人原创, 原发布于CSDN, 在搭建个人博客后使用爬虫批量爬取并挂到个人博客, 出于一些技术原因博客未能完全还原到初始版本(而且我懒得修改), 在观看体验上会有一些瑕疵 ,若有需求会发布重制版总结性新博客。发布时间统一定为1111年11月11日。钦此。


    def initial(req = False):
        global order, vis , arr_lft, arr_need, arr_max, arr_alc, cnt
        MAX = [
            [0,0,4,4]
            ,[2,7,5,0]
            ,[3,6,10,10]
            ,[0,9,8,4]
            ,[0,6,6,10]
        ]
        ALC = [
            [0,0,3,2]
            ,[1,0,0,1]
            ,[1,3,5,4]
            ,[0,3,3,2]
            ,[0,0,1,4]
        ]
        NEED = [1,6,2,2]
        cnt = 0
        arr_lft = np.array(NEED)
        arr_max = np.array(MAX)
        arr_alc = np.array(ALC)
        arr_need = arr_max - arr_alc
        arr_need[arr_need < 0] = 0
        vis = [False for i in range(5)]
        order = []
        if req == True:
            arr_lft -= np.array([1,2,2,2])
            arr_alc[2] += np.array([1,2,2,2])
    def dfs():
        global order, vis , arr_lft, arr_need, arr_max, arr_alc, cnt
        if vis.count(False) == 0:
            print(order)
            cnt += 1
            return
        for i in range(len(vis)):
            if vis[i] == False and True not in (arr_lft - arr_need[i] < 0 ):
                vis[i] = True
                order.append(i)
                arr_lft += arr_alc[i]
                dfs()
                vis[i] = False
                arr_lft -= arr_alc[i]
                order.remove(i)
    def solve(req = False):
        initial(req)
        dfs()
        if cnt != 0: 
            print('共有序列',cnt,'种')
        else:
            print('死锁')
    def main():
        print('第一问:')
        solve()
        print('\n第二问')
        solve(True)
    if __name__ == '__main__':
        main()


样例为：《计算机操作系统》——翟一鸣等老师 课后题2.13

> 第一问:  
>  [0, 3, 1, 2, 4]  
>  [0, 3, 1, 4, 2]  
>  [0, 3, 4, 1, 2]  
>  共有序列 3 种  
>  空格  
>  第二问  
>  死锁

