---
title: CF997E Good Subsegments
tags:
  - 线段树
  - OI
categories:
  - [OI, 数据结构, 线段树]
mathjax: true
date: 2020-09-10 18:40:42
---

一道小有思维难度的黑题。

<!--more-->

原题链接：[CF997E Good Subsegments](https://www.luogu.com.cn/problem/CF997E)

弱化版：[CF526F Pudding Monsters](https://www.luogu.com.cn/problem/CF526F)

模拟赛版：[奇袭](../20200726-mo-ni-sai/#奇袭)

# Good Subsegments

## 题意翻译

有一个$1-n$的**排列**$P$ $(1\le n\le 1.2*10^5)$ 

如果区间$[l,r]$中的数是连续的，那么我们称它为好区间。 

例如，[1, 3, 2, 5, 4]中的好区间有：[1,1], [1, 3], [1, 5], [2, 2], [2, 3], [2, 5], [3, 3], [4, 4], [4, 5], [5, 5]. 

有$q$次询问，每次问$[l,r]$内，有多少子区间是好的？ $(1\le n,q\le 1.2*10^5)$ 

- 输入 

  第一行 $n$ 表示排列的长度 

  第二行 $p_i$ 表示排列 

  第三行 $q$ 表示询问数量 

  接下来 $q$行$l_i,r_i$表示询问 

- 输出 

  $q$行，表示答案。

## 题目描述

A permutation $ p $ of length $ n $ is a sequence $ p_1, p_2, \ldots, p_n $ consisting of $ n $ distinct integers, each of which from $ 1 $ to $ n $ ( $ 1 \leq p_i \leq n $ ) .

Let's call the subsegment $ [l,r] $ of the permutation good if all numbers from the minimum on it to the maximum on this subsegment occur among the numbers $ p_l, p_{l+1}, \dots, p_r $ .

For example, good segments of permutation $ [1, 3, 2, 5, 4] $ are: 

- $ [1, 1] $ , 
- $ [1, 3] $ , 
- $ [1, 5] $ ,
- $ [2, 2] $ , 
- $ [2, 3] $ ,
- $ [2, 5] $ , 
- $ [3, 3] $ ,
- $ [4, 4] $ , 
- $ [4, 5] $ ,
- $ [5, 5] $ . 

You are given a permutation $ p_1, p_2, \ldots, p_n $ .

You need to answer $ q $ queries of the form: find the number of good subsegments of the given segment of permutation.

In other words, to answer one query, you need to calculate the number of good subsegments $ [x \dots y] $ for some given segment $ [l \dots r] $ , such that $ l \leq x \leq y \leq r $ .

## 输入输出格式

### 输入格式

The first line contains a single integer $ n $ ( $ 1 \leq n \leq 120000 $ ) — the number of elements in the permutation.

The second line contains $ n $ distinct integers $ p_1, p_2, \ldots, p_n $ separated by spaces ( $ 1 \leq p_i \leq n $ ).

The third line contains an integer $ q $ ( $ 1 \leq q \leq 120000 $ ) — number of queries.

The following $ q $ lines describe queries, each line contains a pair of integers $ l $ , $ r $ separated by space ( $ 1 \leq l \leq r \leq n $ ).

### 输出格式

Print a $ q $ lines, $ i $ -th of them should contain a number of good subsegments of a segment, given in the $ i $ -th query.

## 输入输出样例

### 输入样例 #1

```
5
1 3 2 5 4
15
1 1
1 2
1 3
1 4
1 5
2 2
2 3
2 4
2 5
3 3
3 4
3 5
4 4
4 5
5 5
```

### 输出样例 #1

```
1
2
5
6
10
1
3
4
7
1
2
4
1
3
1
```

# Solution

这题的题解看了好长时间才看懂。先不说这题，先看看弱化版的题：只有一个询问，询问的是整个区间。

手模不难发现，好区间等价于问区间内最大值减去最小值等于区间长度减一，也就是 $\max-\min=r-l$。

接着手模还可以发现，如果不是好区间，那么要么最大值太大，要么最小值太小，所得出的值 $\max-\min-r+l$ 是一定大于零的。

所以好区间的 $\max-\min-r+l$ 一定是零，而且是所有区间内的最小值，这就为我们维护这东西提供了突破口。如果维护的一堆 $\max-\min-r+l$ 中最小值是零，那么答案就是最小值的个数。

以下将 $\max-\min-r+l$ 称为 $val$。

现在再考虑怎么维护这一堆东西的最小值及其个数。

一个常见的维护区间信息的方式就是固定左端点或者右端点，然后另一个端点遍历所有可能的地方，统计这些区间的答案，然后再移动固定的端点，接着统计，这样可以做到不重不漏统计所有区间对答案的贡献。

所以这道题不妨固定右端点。

维护的这个东西是可以非常简单地区间合并的（具体一会看代码），所以就考虑线段树咯。

因为固定右端点统计左端点在不同地方的贡献，那么可以让线段树上的区间 $[L,R]$ 维护 $[L,r],[L+1,r],\dots,[R,r]$ 这些区间的最小值和最小值个数，而每个叶子节点 $L=R$ 要维护的，就是从 $L$ 到 $r$ 这个区间的信息了。

我们发现右端点向右移动的时候，每个可能区间的 $val$ 都减了一，所以在移动右端点前直接把 $[1,n]$ 所有最小值都减一即可（不懂可以看下面的过程）。

那么再考虑如何把 $\max$ 和 $\min$ 计入，我们来模拟一下这个线段树维护信息的过程。

以以下数据为例：

```
1 3 2 5 4
```

首先把每个线段树区间 $val$ 值设置为左端点 $L$ 的值（马上就会看到为什么要这么做）。（实际上只有 $L\le r$ 的区间可能有贡献，都赋上初值是为了之后方便）

然后建两个单调栈，一个记录单调上升，一个记录单调下降（每一步必须加入当前的数）。

然后右端点 $r$ 开始移动：

- $r$ 移动到 $1$

  将所有 $val$ 都减掉 $1$，也就是 $\max-\min-r+l$ 中 $-r$ 减小了 $1$。

  发现在 $[1,1]$ 区间内出现了一个最大值、一个最小值，但是不会对 $val$ 造成任何影响，先暂且把所有区间看做 $\max=\min=1$，加入单调栈。

  统计 $[1,r]$ 内所有最小值为 $0$ 的区间的个数，发现 $[1,1]$ 是符合条件的，答案加 $1$。

- $r$ 移动到 $2$

  将所有 $val$ 都减掉 $1$，这时候就能看到为什么线段树区间 $val=L$ 了，$r=2$ 对应了移动两次，也就是 $val$ 减了 $2$。$L=R=2$ 的这个区间现在维护 $[2,2]$ 的信息，$val$ 正好应该是 $0$。所以初始化 $val=L$ 是为了保证在移动过后统计的 $val$ 值正确。

  单调下降的那个栈要把栈顶弹掉，所以它对什么区间造成了影响呢？对于 $L=R=r$ 始终有 $\max=\min$，然而现在线段树上 $L=R=1$ 这个区间维护的信息就不对了，因为它维护的是 $[1,2]$ 这个区间，而这个区间还认为 $\max=\min=1$ 呢，它的 $val$ 在之前的 $\max=\min=1$ 的基础上要增加 $3-1=2$。（栈内现在是`3`）

  单调增的直接加进来就可以了。（栈内现在是`1 3`）

  询问有一个 $[2,2]$ 符合，答案加一。

- $r$ 移动到 $3$

  将所有 $val$ 都减掉 $1$。

  发现单调上升的那个单调栈需要弹掉栈顶，在 $L=R=2$ 这个区间的 $val$ 减去 $1$，因为之前维护的是 $[2,2]$，现在变成 $[2,3]$ 了，最小值减小了 $1$，所以这个 $val$ 减一。（栈内现在是`1 2`）

  单调减的直接加入。（栈内现在是`3 2`）

  询问 $[1,3],[2,3],[3,3]$ 都符合，答案加 $3$。

- $r$ 移动到 $4$

  将所有 $val$ 都减掉 $1$。

  单调增的加入。栈内现在是`1 2 5`。

  单调减的弹栈顶，$L=R=3$ 维护的区间 $[3,4]$ 最大值（以及 $val$）增加 $5-2=3$。

  然后发现还不够，再弹栈顶，$L=1,R=2$ 维护的区间最大值增加 $5-3=2$。栈内现在是`5`。

  询问 $[4,4]$ 符合，答案加 $1$。

- $r$ 移动到 $5$

  将所有 $val$ 都减掉 $1$。

  单调增的弹栈顶，$L=R=4$ 维护的区间 $val$ 最小值少了 $5-4=1$。栈内现在是`1 2 4`。

  单调减的栈直接加入，现在是`5 4`。

  询问 $[1,5],[2,5],[4,5],[5,5]$ 符合，答案加 $4$。

呼~

所以通过手动模拟，可以发现单调栈模拟的是 新加元素 对之前区间维护的最大或最小值 的影响，而要改动的那些影响，就要把栈顶控制的区域更新，而这个区域，就是栈顶和栈内第二个元素下标之间的部分。

所以代码也就呼之欲出了。

而若想更高效地统计答案（或者多测），可以考虑之前的历史区间对右端点移动后的贡献。

线段树每个节点多维护一个 $ans$ 和 $lzAns$，表示这个区间当前的答案和答案累加的懒标记。



