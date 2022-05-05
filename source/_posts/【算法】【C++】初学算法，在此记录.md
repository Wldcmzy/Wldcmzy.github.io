---
title: 【算法】【C++】初学算法，在此记录.md
date: 1111-11-11 11:11:11
categories:
  - OldBlog(Before20220505)
tags:
  - OldBlog(Before20220505)
---

## 说明 - 2022-05-05
本篇博客为本人原创, 原发布于CSDN, 在搭建个人博客后使用爬虫批量爬取并挂到个人博客, 出于一些技术原因博客未能完全还原到初始版本(而且我懒得修改), 在观看体验上会有一些瑕疵 ,若有需求会发布重制版总结性新博客。发布时间统一定为1111年11月11日。钦此。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210416214105534.png#pic_center)

## 0.预留位置

## 1.使用Manacher算法求最长回文子串

给出一个字符串，要求计算出这一字符串的最长回文子串的长度。  
如果遍历每一个字符，并以该字符为中心向两边查找，则其时间  
复杂度为O(n2)。  
Manacher算法，又被戏称为“马拉车”算法，可以在时间复杂度  
为O(n)的情况下求解一个字符串的最长回文子串的长度。

###### 1.

回文串分为奇回文（abcba）和偶回文（baccba），为了避免区分奇偶的麻烦，把字符串变一下形式，在字符串之间加入特殊符号，并且
**首位再加不同的特殊符号以防越界** ，于是字符串变成奇回文（上述偶回文得到的新字符串：@#a#b#c#c#b#a!）

###### 2.

给得到的新字符串str_new定义一个辅助数组int p[]，p[i]表示在str_new中以第i个位置为中心得到的最长回文串的 **半径**

###### 3.

可想而知，若新字符串str_new的最大半径为p[i]，则原字符串的最长回文子串长度为p[i]-1

###### 4.![在这里插入图片描述](https://img-blog.csdnimg.cn/20200722184021570.PNG?x-oss-
process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dsZGNtenk=,size_16,color_FFFFFF,t_70)

对于p[i]，如果i<mx，设j是i关于id对称点，如图所示，则基于以下三  
种情况，可以求出p[i]的值：  
（1）以j为中心的回文串有一部分在以id为中心的回文串之外。因为mx  
是以id为中心的最长回文的右边界，所以以i为中心的回文串不可能会  
有字符在以id为中心的回文串之外；否则mx就不是以id为中心的最长回  
文的右边界。所以，在这种情况下，p[i]=mx–i。  
（2）以j为中心的回文串全部在以id为中心的回文串的内部，则  
p[i]=p[j]，而且p[i]不可能再增加。  
（3）以j为中心的回文串的左端正好与以id为中心的回文串的左端重合。  
则p[i]=p[j]或p[i]=mx–i，并且p[i]还有可能会继续增加，即while  
(str_new[i-p[i]]==str_new[i+p[i]]) p[i]++;

###### 5.

从str[1]开始遍历到末尾  
先通过

    
    
    if(i<mx)    p[i]=std::min(p[2*id-i],mx-i);
    else        p[i]=1;
    

确定好id-mx范围内的以i为中心的最长子串半径，然后再通过

    
    
    while(str[i+p[i]]==str[i-p[i]])     p[i]+=1;
    

确定最长半径

若得到以i为中心的子串最右位置超过mx，则更新范围

    
    
    if(p[i]+i>mx)           //超出最右边就更新范围
            {
                id=i;
                mx=p[i]+i;
                mxsub=std::max(mxsub,p[i]);     //更新最长子串长度
            }
    

###### 代码：输入一个字符串，用Manacher算法求其中最长回文子串的长度

    
    
    #include<iostream>
    #include<cstring>
    #include<cstdio>
    #include<algorithm>
    const int max_len = 1e6+3;      //输入字符串的最大长度
    const int inPos=max_len+2;      //字符串输入位置，从str数组中间输入字符串
    int p[max_len*2];               //p[i]:以i为中心的回文串的最大长度
    int len;
    char str[max_len*2];            //用于储存输入的字符串
    
    //把字符串各字符之间插入字符，首位再各插入不同字符防止越界
    void change_str()
    {
        int index=0;
        len=strlen(str);            //获得输入字符串的长度
        str[index++]='@';
        for(int i=inPos; i<len; i++)
        {
            str[index++]='#';
            str[index++]=str[i];
        }
        str[index++]='#';
        str[index]='!';
        len=index;                  //获得改造后字符串的长度
    }
    
    int manacher()
    {
        int mx=0,id=1;              //mx表示最右端位置 id表示中心点位置
        int mxsub=0;                //最长子串长度
        for(int i=1; i<len; i++)
        {
            if(i<mx)    p[i]=std::min(p[2*id-i],mx-i);
            else        p[i]=1;
            while(str[i+p[i]]==str[i-p[i]])     p[i]+=1;
            if(p[i]+i>mx)           //超出最右边就更新范围
            {
                id=i;
                mx=p[i]+i;
                mxsub=std::max(mxsub,p[i]);     //更新最长子串长度
            }
        }
        return mxsub-1;             //返回结果
    }
    
    int main()
    {
        memset(str,' ',sizeof(str));
        scanf("%s",str+inPos);
        change_str();
        std::cout << manacher() << std::endl;
        return 0;
    }
    
    

## 2.用树结构支持并查集

###### 并查集：

在一些应用中，需要把n个不同元素划分成不相交的若干组，每  
一组的元素构成一个集合，由于这类问题主要涉及对集合的合  
并和查找，因此称为并查集。  
并查集维护一些互不相交的集合S={S1  
, S2  
, …, Sr}，每个集合Si  
都有一个特殊元素rep[Si]，称为集合的代表元。

###### 并查集的三种操作：

Make_Set(x)：加入一个含单元素x的集合{x}到并查集S，且rep[{x}]=x。 x不能被包含在任何一个Si中, 因为S里任何两个集合是不相交的。  
初始时，对每个元素x执行一次Make_Set(x)。

join(x, y)：把x和y所在的两个不同集合Sx和Sy  
合并：从S中删除Sx和Sy，  
并加入Sx与Sy的并集。

set_find(x)：返回x所在集合Sx  
的代表元rep[Sx]。

###### 树结构

每个集合用一棵树表示, 根为集合的代表元。每个节点p设一个指针(抄的原话，看起来不像指针)  
set[p]，记录它所在树的根节点序号。如果set[p]<0，则表明p为根节点。  
初始时，为每一个元素建立一个集合，即 set[x]=-1（1≤x≤n）。

###### 树结构查找操作set_find(x) ：边查找边“路径压缩”

首先，从节点x出发，沿set指针查找节点x所在树的根节点f（set[f]<0）。  
然后，进行路径压缩，将x至f的路径上经过的每个节点的set指针都指向f。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200722203030627.PNG?x-oss-
process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dsZGNtenk=,size_16,color_FFFFFF,t_70)

###### 树结构查找操作：

int set_find(int p) // 查找p所在集合的代表元，用路径压缩优化  
{  
if (set[p]<0)  
return p;  
return set[p]=set_find(set[p]);  
}

###### 树结构合并操作join(x, y) ：将两棵树的根节点相连

计算x元素所在并查集的树根fx和y元素所在并查集的树根fy。如果fx==fy，则说  
明元素x和元素y在同一并查集中；否则将x所在的集合并入y所在的集合，也就  
是将fx的set指针设为fy。

###### 题目：Find them, Catch them

> The police office in Tadu City decides to say ends to the chaos, as  
>  launch actions to root up the TWO gangs in the city, Gang Dragon and  
>  Gang Snake. However, the police first needs to identify which gang a  
>  criminal belongs to. The present question is, given two criminals; do  
>  they belong to a same clan? You must give your judgment based on  
>  incomplete information. (Since the gangsters are always acting  
>  secretly.)
>
> Assume N (N <= 10^5) criminals are currently in Tadu City, numbered  
>  from 1 to N. And of course, at least one of them belongs to Gang  
>  Dragon, and the same for Gang Snake. You will be given M (M <= 10^5)  
>  messages in sequence, which are in the following two kinds:
>
> D [a] [b] where [a] and [b] are the numbers of two criminals, and they  
>  belong to different gangs.
>
> A [a] [b] where [a] and [b] are the numbers of two criminals. This  
>  requires you to decide whether a and b belong to a same gang. Input  
>  The first line of the input contains a single integer T (1 <= T <=  
>  20), the number of test cases. Then T cases follow. Each test case  
>  begins with a line with two integers N and M, followed by M lines each  
>  containing one message as described above. Output For each message “A  
>  [a] [b]” in each case, your program should give the judgment based on  
>  the information got before. The answers might be one of “In the same  
>  gang.”, “In different gangs.” and “Not sure yet.”
>
> Sample Input  
>  1  
>  5 5  
>  A 1 2  
>  D 1 2  
>  A 1 2  
>  D 2 4  
>  A 1 4  
>  Sample Output Not  
>  sure yet.  
>  In different gangs.  
>  In the same gang.

翻译一下：

> Tadu市的警察局决定结束混乱，因此要采取行动，根除城市中的两大 帮派：龙帮和蛇帮。然而，警方首先需要确定某个罪犯是属于哪个帮派。  
>  目前的问题是，给出两个罪犯，他们是属于同一个帮派吗？您要基于不 完全的信息给出您的判断，因为歹徒总是在暗中行事。  
>  假设在Tadu市现在有N（N≤105 ）个罪犯，编号从1到N。当然，至少有 一个罪犯属于龙帮，也至少有一个罪犯属于蛇帮。给出M（M≤105  
>  ）条 消息组成的序列，消息有下列两种形式：  
>  D [a] [b] 其中[a]和[b]是两个犯罪分子的编号，他们属于不同的帮派；  
>  A [a] [b] 其中[a]和[b]是两个犯罪分子的编号，您要确定a和b是否属于同一帮派。 输入
> 输入的第一行给出给出一个整数T（1≤T≤20），表示测试用例的个 数。后面跟着T个测试用例，每个测试用例的第一行给出两个整数  
>  N和M，后面的M行每行给出一条如上面所描述的消息。 输出 对于在测试用例中的每条“A [a] [b]”消息，您的程序要基于此前  
>  给出的信息做出判断。回答是如下之一 “In the same gang.”， “In different gangs.”或“Not sure  
>  yet.”。

###### 解题思路：

1.建立一个数组int
set[]其大小大于等于罪犯数的两倍，把数组初始化为-1，即数组的每个元素代表一个独立的集合。把set[i+n]看作是集合set[i]的对立集合(n为罪犯数目)  
2.遇到D[a]
[b]语句，说明a和b不能在同一个集合，就把a所在的集合的值set[a]赋值为b的对立集合set[b+n]对应的下标b+n，把b所在的集合的值赋值给a的对立集合的下标a+n  
每次遇到D[a] [b]语句都进行如此操作，便可以做到集合合并（相互链接的就算一个集合了）  
3.遇到A[a] [b]语句，查询（参照路径压缩的思路）a、b所在的集合与b所在的集合相同，输出 “In the same gang.”  
，若a、b所在的集合不同，而且a不在b的对立集合，输出“Not sure  
yet.”，否则输出 “In different gangs.”

###### 代码：

    
    
    #include<iostream>
    #include<cstring>
    #include<cstdio>
    #include<algorithm>
    const int max_criminal = 1e5+5;
    int sett[max_criminal*2];           //建立多于罪犯总数两倍个集合
    
    //查找元素所在的集合(顺便压缩路径)
    int set_find(int index)
    {
        if(sett[index]<0)
            return index;
        return sett[index]=set_find(sett[index]);
    }
    
    int main()
    {
        int t,n,m,a,b;
        char ch;
        std::cin >> t;
        while(t-- && std::cin >> n >> m)
        {
            memset(sett,-1,sizeof(sett));
            for(int i=0; i<m; i++)
            {
                std::cin >> ch >> a >> b;
                if(ch=='D')
                {
                    sett[set_find(a)]=set_find(b+n);
                    sett[set_find(b)]=set_find(a+n);
                }
                else
                {
                    if(set_find(a)==set_find(b))
                        std::cout << "In the same gang." << std::endl;
                    else if(set_find(a)==set_find(b+n))
                        std::cout << "In different gangs." << std::endl;
                    else
                        std::cout << "Not sure yet." << std::endl;
                }
            }
        }
        return 0;
    }
    

## 3.应用Trie树查询字符串

###### 定义（Trie树）

Trie树，也被称为单词查找树，前缀树，或字典树。  
其基本性质如下：  
• 1，根节点不包含字符，除根节点外，每个节点只包含一个字符。  
• 2，将从根节点到某一个节点的路上经过的节点所包含的字符连  
接起来，就是该节点对应的字符串。  
• 3，对于每个节点，其所有子节点包含的字符是不相同的。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200723171651441.PNG?x-oss-
process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dsZGNtenk=,size_16,color_FFFFFF,t_70)  
Trie树的根节点root对应空字符串。一般情况下，不是所有的节点  
都有对应的值，只有叶节点和部分内节点所对应的字符串才有相  
关的值。所以，Trie树是一种用于快速检索的多叉树结构，每个  
节点保存一个字符，一条路可以用于表示一个字符串、一个电话  
号码等等信息。

###### 题目：Shortest Prefixes

字符串的前缀是从给出的字符串的开头开始的子字符串。  
“carbon"的前缀是：“c”，“ca”，“car”，“carb”，“carbo"和"carbon”。  
在本题中，空串不被视为前缀，但是每个非空字符串都被视为其  
自身的前缀。在日常语言中，我们会用前缀来缩写单词。例如，  
“carbohydrate” （“碳水化合物”）通常被缩写为"carb”。在本题  
中，给出一组单词，请您为每个单词找出能唯一标识该单词的最  
短前缀。  
• 在给出的样例输入中，“carbohydrate"可以被缩写为"carboh”，但  
不能被缩写为"carbo"（或者更短），因为有其他单词以"carbo"开  
头。  
• 完全匹配也可以作为前缀匹配。例如，给出的单词"car"，其前缀  
"car"与"car"完全匹配。因此，"car"是"car"的缩写，而不是  
"carriage"或列表中以"car"开头的其他任何其他词的缩写。

• **输入**  
• 输入至少有两行，最多不超过1000行。每行给出一个由1到20  
个小写字母组成的单词。

• **输出**  
• 输出的行数与输入的行数相同。输出的每一行先给出输入对应行  
中的单词，然后给出一个空格，最后给出唯一（无歧义）标识该  
单词的最短前缀。

###### 代码（可能有问题）：

    
    
    #include <iostream>
    #include <cstdio>
    #include <cstring>
    #include <algorithm>
    int len,idx,letterIdx;             //letterIdx表示英文字母按字母表的序号（0-25）
    const int max_node = 1000*22;       //最大节点数
    char save_str[1001][22];            //用于保存字符串
    int Node[max_node][26];             //提前给节点开出空间来
    int vis[max_node];                  //节点访问次数
    
    struct Trie
    {
        int tidx;                       //目前第一个尚未使用的节点序号（到目前位置一共使用了多少节点）
        Trie()
        {
            tidx=1;
            memset(Node,0,sizeof(Node));
        }
    
        //得到字母序号letterIdx
        int getletterIdx(char ch)
        {
            return ch-'a';
        }
    
        void insert(char * str)
        {
            idx=0,len=strlen(str);
            for(int i=0; i<len; i++)
            {
                letterIdx=getletterIdx(str[i]);
                if(Node[idx][letterIdx]==0)//如果此节点应该对应的下一个解点还没有给出，则创建这个解点
                {
                    vis[tidx]=0;
                    Node[idx][letterIdx]=tidx++;
                }
                idx=Node[idx][letterIdx];   //序号idx更新为的下一个节点的序号
                vis[idx]++;                 //访问次数+1
            }
        }
    
        void search(char * str)
        {
            idx=0,len=strlen(str);
            for(int i=0; i<len; i++)
            {
                putchar(str[i]);
                letterIdx=getletterIdx(str[i]);
                if(vis[Node[idx][letterIdx]]<=1) //如果下一个节点访问次数仅为1，则只用之前的节点就可以确定唯一单词
                {
                    return ;
                }
                idx=Node[idx][letterIdx];       //序号idx更新为下一个节点的序号
    
            }
        }
    };
    
    int main()
    {
        int t,strIdx=0;
        Trie trie;
        std::cin >> t;
        for(int i=0; i<t; i++)
        {
            scanf("%s",save_str[strIdx]);
            trie.insert(save_str[strIdx]);
            strIdx+=1;
        }
        for(int i=0; i<t; i++)
        {
            std::cout << save_str[i] << " ";
            trie.search(save_str[i]);
            std::cout << std::endl;
        }
    
        return 0;
    }
    

这个解法必须建立在所有输入没有重复的前提上，有的话要另外加对应的处理。

## 4.使用KMP算法匹配字符串

Knuth-Morris-Pratt 字符串查找算法，简称为 “KMP算法”，常用于在一个文本串S内查找一个模式串P 的出现位置，这个算法由Donald
Knuth、Vaughan Pratt、James H. Morris三人于1977年联合发表，故取这3人的姓氏命名此算法。

以下为个人理解

###### 大概思路：

为了避免逐位查找带来的时间浪费，KMP算法使用一个数组 int
next[]记录一个字符串从第0位到第i位相同的前缀和后缀长度（或者此长度-1，都可以），查询时可以跨指定长度匹配，这个next数组怎么求稍后说。这样可以提高匹配速度。  
**比如：** 字符串 **str1：abcababcabca** 与字符串 **str2：abcabc**
匹配，str2的next数组为{-1，-1，-1，0，1，2}。两字符串匹配到str1[4]与str2[4]完全相同，第五位不同，不完全相同。下一次匹配不需要逐位匹配（从str1[1]与str[0]开始匹配），而是尝试从str1[5]与str2[next[4]+1]匹配

###### next数组求法：

对所查询字符串求next数组，第一位不管（一般为0或-1），从第二位开始，根据以前的信息，如果上一位前后缀匹配，则直接判断下一位是否匹配，若下一位也匹配，长度求出，若下一位不匹配，则在目前已经匹配了的若干为中，寻找最长相同前后缀

###### 代码：（匹配一个字符串在另一个字符串中的出现次数）

    
    
    #include <iostream>
    #include <cstdio>
    #include <cstring>
    #include <algorithm>
    char str1[100005],str2[1005];
    int next[1005];
    
    void getNext()
    {
        next[0]=-1;
        int len=strlen(str2),k;
        for(int i=1; i<len ;i++)
        {
            k=next[i-1];
            while(str2[k+1]!=str2[i] && k>=0) k=next[k];
            if(str2[k+1]==str2[i]) next[i]=k+1;
            else next[i]=-1;
        }
    }
    int kmp()
    {
        int counter=0,i=0,j=0,len=strlen(str1),sublen=strlen(str2);
        while(i<len && j<sublen)
        {
            if(str1[i]==str2[j])
            {
                i+=1,j+=1;
                if(j==sublen)
                {
                    counter+=1;
                    j=next[j-1]+1;
                }
            }
            else
            {
                if(j==0) i+=1;
                else j=next[j-1]+1;
            }
        }
        return counter;
    }
    
    int main()
    {
        int t;
        std::cin >> t;
        std::cin >> str1;
        while(t--)
        {
            std::cin >> str2;
            getNext();
            std::cout << kmp() << std::endl;
        }
        return 0;
    }
    
    

