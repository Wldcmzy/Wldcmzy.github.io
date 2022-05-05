---
title: 【AC自动机 _ trie树】G - Word Puzzles   POJ - 1204.md
date: 1111-11-11 11:11:11
categories:
  - OldBlog(Before20220505)
tags:
  - OldBlog(Before20220505)
---

## 说明 - 2022-05-05
本篇博客为本人原创, 原发布于CSDN, 在搭建个人博客后使用爬虫批量爬取并挂到个人博客, 出于一些技术原因博客未能完全还原到初始版本(而且我懒得修改), 在观看体验上会有一些瑕疵 ,若有需求会发布重制版总结性新博客。发布时间统一定为1111年11月11日。钦此。

## 题目描述

Word puzzles are usually simple and very entertaining for all ages. They are
so entertaining that Pizza-Hut company started using table covers with word
puzzles printed on them, possibly with the intent to minimise their client’s
perception of any possible delay in bringing them their order.

Even though word puzzles may be entertaining to solve by hand, they may become
boring when they get very large. Computers do not yet get bored in solving
tasks, therefore we thought you could devise a program to speedup (hopefully!)
solution finding in such puzzles.

The following figure illustrates the PizzaHut puzzle. The names of the pizzas
to be found in the puzzle are: MARGARITA, ALEMA, BARBECUE, TROPICAL, SUPREMA,
LOUISIANA, CHEESEHAM, EUROPA, HAVAIANA,
CAMPONESA.![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly92ai56MTgwLmNuL2QwYzAxMDE5MTcwNDBkMTY2NTgzYmE5YzNhYWQ4Yzkz?x-oss-
process=image/format,png)  
Your task is to produce a program that given the word puzzle and words to be
found in the puzzle, determines, for each word, the position of the first
letter and its orientation in the puzzle.

You can assume that the left upper corner of the puzzle is the origin, (0,0).
Furthemore, the orientation of the word is marked clockwise starting with
letter A for north (note: there are 8 possible directions in total).

## 输入

The first line of input consists of three positive numbers, the number of
lines, 0 < L <= 1000, the number of columns, 0 < C <= 1000, and the number of
words to be found, 0 < W <= 1000. The following L input lines, each one of
size C characters, contain the word puzzle. Then at last the W words are input
one per line.

## 输出

Your program should output, for each word (using the same order as the words
were input) a triplet defining the coordinates, line and column, where the
first letter of the word appears, followed by a letter indicating the
orientation of the word according to the rules define above. Each value in the
triplet must be separated by one space only.

## 输入样例

20 20 10  
QWSPILAATIRAGRAMYKEI  
AGTRCLQAXLPOIJLFVBUQ  
TQTKAZXVMRWALEMAPKCW  
LIEACNKAZXKPOTPIZCEO  
FGKLSTCBTROPICALBLBC  
JEWHJEEWSMLPOEKORORA  
LUPQWRNJOAAGJKMUSJAE  
KRQEIOLOAOQPRTVILCBZ  
QOPUCAJSPPOUTMTSLPSF  
LPOUYTRFGMMLKIUISXSW  
WAHCPOIYTGAKLMNAHBVA  
EIAKHPLBGSMCLOGNGJML  
LDTIKENVCSWQAZUAOEAL  
HOPLPGEJKMNUTIIORMNC  
LOIUFTGSQACAXMOPBEIO  
QOASDHOPEPNBUYUYOBXB  
IONIAELOJHSWASMOUTRK  
HPOIYTJPLNAQWDRIBITG  
LPOINUYMRTEMPTMLMNBO  
PAFCOPLHAVAIANALBPFS  
MARGARITA  
ALEMA  
BARBECUE  
TROPICAL  
SUPREMA  
LOUISIANA  
CHEESEHAM  
EUROPA  
HAVAIANA  
CAMPONESA

## 输出样例

0 15 G  
2 11 C  
7 18 A  
4 8 C  
16 13 B  
4 15 E  
10 3 D  
5 1 E  
19 7 C  
11 11 H

## 瞎翻译

给你一张字谜图，里头有一堆乱七八糟的字母，  
**输入：**  
先输入仨数L,C,W，前两个(L,C)是字谜图的行数和列数，第三个(W)是输入单词的数目，  
之后输入L行C列的数填满字谜图，  
最后W行输入W个字符串，这W个字符串肯定都可以在字谜图上找到（字母横着竖着或斜着线性连续）  
**输出：**  
一行输出三个元素，前两个是找到的字符串的首字母的坐标，第三个是字符串的排列方向（从首字母到尾字母的方向），方向一共有8个，按照顺时针旋转，A代表正上方，B代表右上方，一次类推

## 使用trie树暴力破解

**思路：** 用所有所查询的字符串建立一颗trie树，然后遍历地图上的每个点，在每个点分别朝8个方向对trie树搜索（三重循环无脑搜就完了）


​    
```cpp
#include <iostream>
#include <cstdio>
#include <cstring>
#include <queue>

const int dy[8]={0,1,1,1,0,-1,-1,-1};//设左上角为原点，向下为x轴正方向，向右为y轴正方向
const int dx[8]={-1,-1,0,1,1,1,0,-1};//先设置好移动方向，dx[0],dy[0]为向上方向（A方向），方向按照顺时针旋转顺序写
const int maxn=1005;
char cell[maxn][maxn],word[maxn];//cell储存字谜地图，word用于接收一个要搜索的单词
int slen[maxn],tidx,r,c,w,ans[maxn][3];//slen储存每个要搜索的单词的长度，tidx为节点序号，ans储存答案（方向+横纵坐标）
struct Node{
    int id,next[26];//在trie树中每个单词的结尾给id赋值，其值为 这个单词按照输入顺序排的序号（从1开始，不是0）
}treeN[maxn*maxn];
```


​    
```cpp
//每输入一个单词，进行一次此函数，把单词存入trie树
void joinTree(int id)
{
    int root=0,len=strlen(word),nxt;
    slen[id]=len;       //把此字符串的长度储存在len数组中
    for(int i=0; i<len; i++){
        nxt=word[i]-'A';
        if(treeN[root].next[nxt]<=0)
            treeN[root].next[nxt]=++tidx;
        root=treeN[root].next[nxt];
    }
    treeN[root].id=id;//在trie树中每个单词的结尾给id赋值，其值为 这个单词按照输入顺序排的序号（从1开始，不是0）
}
```


​    
```cpp
//查找函数
void sch(int x,int y,int dir){
    int root=0,id;
    while(x>=0 && x<=r && y>=0 && y<=c){    //在不越界的条件下不断沿trie树搜索
        root=treeN[root].next[cell[x][y]-'A'];
        if(root<=0) break;
        id=treeN[root].id;
        if(id>0){   //若id>0，说明这个位置是第id个单词的结尾，略微处理得到答案，存入ans数组
            ans[id][0]=dir;
            ans[id][1]=x-(slen[id]-1)*dx[dir];
            ans[id][2]=y-(slen[id]-1)*dy[dir];
        }
        x+=dx[dir];
        y+=dy[dir];
    }
}
```


​    
```cpp
int main(){
    tidx=0;
    std::cin >> r >> c >> w;
    for(int i=0; i<r; i++)
        std::cin >> cell[i];
    for(int i=1; i<=w; i++){
        std::cin >> word;
        joinTree(i);
    }
    for(int i=0; i<r; i++)// 输入所有数据并建立好tire树后暴搜得到答案
        for(int j=0; j<c; j++)
            for(int k=0; k<8; k++)
                sch(i,j,k);
    for(int i=1; i<=w; i++)
        printf("%d %d %c\n",ans[i][1],ans[i][2],ans[i][0]+'A');
    return 0;
}
```


​    

## AC自动机解法

在trie树的基础上使用AC自动机（只需要多一步添加fail指针，然后再修改亿点点）  
AC自动机的好处是只需要在地图最外围的一圈把8个方向都遍历一次就行了，因为它不用像单纯用trie树一样每次只能从根节点开始搜索，而是匹配不成功时通过fail指针转移到树的其他位置继续搜索


​    
```cpp
#include <iostream>
#include <cstdio>
#include <cstring>
#include <queue>
//相对于暴搜，用AC自动机在地图最外围的一圈把8个方向都遍历一次

const int dy[8]={0,1,1,1,0,-1,-1,-1};
const int dx[8]={-1,-1,0,1,1,1,0,-1};
const int maxn=1005;
char cell[maxn][maxn],word[maxn];
int slen[maxn],tidx,r,c,w,ans[maxn][3];
struct Node{
    int id,next[26],fail;
}treeN[maxn*maxn];
void joinTree(int id)
{
    int root=0,len=strlen(word),nxt;
    slen[id]=len;
    for(int i=0; i<len; i++){
        nxt=word[i]-'A';
        if(treeN[root].next[nxt]<=0)
            treeN[root].next[nxt]=++tidx;
        root=treeN[root].next[nxt];
    }
    treeN[root].id=id;
}

//建立好trie树后调用此函数通过bfs添加fail指针
void getFail()
{
    int nxt,cur,fail;
    std::queue<int> q;
    for(int i=0; i<26; i++){
        nxt=treeN[0].next[i];
        if(nxt>0){
            treeN[nxt].fail=0;
            q.push(nxt);
        }
    }
    while(!q.empty()){
        cur=q.front(),q.pop();
        fail=treeN[cur].fail;
        for(int i=0; i<26; i++){
            nxt=treeN[cur].next[i];
            if(nxt>0){
                treeN[nxt].fail=treeN[fail].next[i];
                q.push(nxt);
            }
            else treeN[cur].next[i]=treeN[fail].next[i];
        }
    }
}

void ACgo(int x,int y,int dir){
    int root=0,id;
    while(x>=0 && x<=r && y>=0 && y<=c){
        while( root>0 && treeN[root].next[cell[x][y]-'A']<=0)//如果不能匹配就通过fail指针回溯，在trie的其他位置继续寻找，节约了时间
            root=treeN[root].fail;
        root=treeN[root].next[cell[x][y]-'A'];
        id=treeN[root].id;
        if(id>0){
            ans[id][0]=dir;
            ans[id][1]=x-(slen[id]-1)*dx[dir];
            ans[id][2]=y-(slen[id]-1)*dy[dir];
        }
        x+=dx[dir];
        y+=dy[dir];
    }
}
int main(){
    tidx=0;
    std::cin >> r >> c >> w;
    for(int i=0; i<r; i++)
        std::cin >> cell[i];
    for(int i=1; i<=w; i++){
        std::cin >> word;
        joinTree(i);
    }
    getFail();
    for(int i=0; i<r; i++)
        for(int j=0; j<8; j++)
            ACgo(i,0,j),ACgo(i,r-1,j);
    for(int i=0; i<c; i++)
        for(int j=0; j<8; j++)
            ACgo(0,i,j),ACgo(c-1,i,j);
    for(int i=1; i<=w; i++)
        printf("%d %d %c\n",ans[i][1],ans[i][2],ans[i][0]+'A');
    return 0;
}
```


​    

## 反向构建trie树，不用记录单词长度

**两段代码都只改了一点，但都时间超限，目前没发现原因，慎看**  
**两段代码都只改了一点，但都时间超限，目前没发现原因，慎看**  
**两段代码都只改了一点，但都时间超限，目前没发现原因，慎看**  
**两段代码都只改了一点，但都时间超限，目前没发现原因，慎看**  
**两段代码都只改了一点，但都时间超限，目前没发现原因，慎看**


​    
```cpp
#include <iostream>
#include <cstdio>
#include <cstring>
#include <queue>
const int dy[8]={0,-1,-1,-1,0,1,1,1};
const int dx[8]={1,1,0,-1,-1,-1,0,1};
const int maxn=1005;
char cell[maxn][maxn],word[maxn];
int tidx,r,c,w,ans[maxn][3];
struct Node{
    int id,next[26],fail;
}treeN[maxn*maxn];
void joinTree(int id)
{
    int root=0,len=strlen(word),nxt;
    for(int i=len-1; i>=0; i--){
        nxt=word[i]-'A';
        if(treeN[root].next[nxt]<=0)
            treeN[root].next[nxt]=++tidx;
        root=treeN[root].next[nxt];
    }
    treeN[root].id=id;
}
void getFail()
{
    int nxt,cur,fail;
    std::queue<int> q;
    for(int i=0; i<26; i++){
        nxt=treeN[0].next[i];
        if(nxt>0){
            treeN[nxt].fail=0;
            q.push(nxt);
        }
    }
    while(!q.empty()){
        cur=q.front(),q.pop();
        fail=treeN[cur].fail;
        for(int i=0; i<26; i++){
            nxt=treeN[cur].next[i];
            if(nxt>0){
                treeN[nxt].fail=treeN[fail].next[i];
                q.push(nxt);
            }
            else treeN[cur].next[i]=treeN[fail].next[i];
        }
    }
}
void ACgo(int x,int y,int dir){
    int root=0,id;
    while(x>=0 && x<=r && y>=0 && y<=c){
        while( root>0 && treeN[root].next[cell[x][y]-'A']<=0)
            root=treeN[root].fail;
        root=treeN[root].next[cell[x][y]-'A'];
        id=treeN[root].id;
        if(id>0){
            ans[id][0]=dir;
            ans[id][1]=x;
            ans[id][2]=y;
        }
        x+=dx[dir];
        y+=dy[dir];
    }
}
int main(){
    tidx=0;
    std::cin >> r >> c >> w;
    for(int i=0; i<r; i++)
        std::cin >> cell[i];
    for(int i=1; i<=w; i++){
        std::cin >> word;
        joinTree(i);
    }
    getFail();
    for(int i=0; i<r; i++)
        for(int j=0; j<8; j++)
            ACgo(i,0,j),ACgo(i,r-1,j);
    for(int i=0; i<c; i++)
        for(int j=0; j<8; j++)
            ACgo(0,i,j),ACgo(c-1,i,j);
    for(int i=1; i<=w; i++)
        printf("%d %d %c\n",ans[i][1],ans[i][2],ans[i][0]+'A');
    return 0;
}
```


​    
​    
​    
​    
```cpp
#include <iostream>
#include <cstdio>
#include <cstring>
#include <queue>

const int dy[8]={0,-1,-1,-1,0,1,1,1};//设左上角为原点，向下为x轴正方向，向右为y轴正方向
const int dx[8]={1,1,0,-1,-1,-1,0,1};//先设置好移动方向，dx[0],dy[0]为向上方向（A方向），方向按照顺时针旋转顺序写
const int maxn=1005;
char cell[maxn][maxn],word[maxn];//cell储存字谜地图，word用于接收一个要搜索的单词
int tidx,r,c,w,ans[maxn][3];//tidx为节点序号，ans储存答案（方向+横纵坐标）
struct Node{
    int id,next[26];//在trie树中每个单词的结尾给id赋值，其值为 这个单词按照输入顺序排的序号（从1开始，不是0）
}treeN[maxn*maxn];
```


​    
```cpp
//每输入一个单词，进行一次此函数，把单词存入trie树
void joinTree(int id)
{
    int root=0,len=strlen(word),nxt;
    for(int i=len-1; i>=0; i--){
        nxt=word[i]-'A';
        if(treeN[root].next[nxt]<=0)
            treeN[root].next[nxt]=++tidx;
        root=treeN[root].next[nxt];
    }
    treeN[root].id=id;//在trie树中每个单词的结尾给id赋值，其值为 这个单词按照输入顺序排的序号（从1开始，不是0）
}
```


​    
```cpp
//查找函数
void sch(int x,int y,int dir){
    int root=0,id;
    while(x>=0 && x<=r && y>=0 && y<=c){    //在不越界的条件下不断沿trie树搜索
        root=treeN[root].next[cell[x][y]-'A'];
        if(root<=0) break;
        id=treeN[root].id;
        if(id>0){   //若id>0，说明这个位置是第id个单词的结尾，略微处理得到答案，存入ans数组
            ans[id][0]=dir;
            ans[id][1]=x;
            ans[id][2]=y;
        }
        x+=dx[dir];
        y+=dy[dir];
    }
}
```


​    
```cpp
int main(){
    tidx=0;
    std::cin >> r >> c >> w;
    for(int i=0; i<r; i++)
        std::cin >> cell[i];
    for(int i=1; i<=w; i++){
        std::cin >> word;
        joinTree(i);
    }
    for(int i=0; i<r; i++)// 输入所有数据并建立好tire树后暴搜得到答案
        for(int j=0; j<c; j++)
            for(int k=0; k<8; k++)
                sch(i,j,k);
    for(int i=1; i<=w; i++)
        printf("%d %d %c\n",ans[i][1],ans[i][2],ans[i][0]+'A');
    return 0;
}
```


​    

