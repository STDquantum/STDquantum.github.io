---
title: 01Trie学习笔记
tags:
  - OI
categories:
  - [OI, 图论, 01Trie]
mathjax: true
date: 2020-07-30 16:03:19
---

有一个东西叫做01Trie，在Trie的思想上把字母边改为$0/1$，用二进制表示的一个数在Trie上跑。

有很多神奇操作（比如当平衡树用）

<!--more-->

# P4551 最长异或路径

链接：[P4551 最长异或路径](https://www.luogu.com.cn/problem/P4551)

## 题目描述

给定一棵$n$个点的带权树，结点下标从$1$开始到$N$。寻找树中找两个结点，求最长的异或路径。 异或路径指的是指两个结点之间唯一路径上的所有边权的异或。

## 输入输出格式

### 输入格式

第一行一个整数$N$，表示点数。 接下来 $n-1$ 行，给出 $u,v,w$ ，分别表示树上的 $u$ 点和 $v$ 点有连边，边的权值是 $w$。

### 输出格式

一行，一个整数表示答案。

## 输入输出样例

### 输入样例 #1

```
4
1 2 3
2 3 4
2 4 6
```

### 输出样例 #1

```
7
```

## 说明

最长异或序列是1-2-3，答案是 7 (=3 ⊕ 4) 

### 数据范围 

$1\le n \le 100000;0 < u,v \le n;0 \le w < 2^{31}$

## 题解

可以看做01Trie的入门题

记录每个节点到根的路径异或和加入01Trie，然后枚举每个节点，再在01Trie上查询能达到的最大值。

代码：

```cpp
#include <bits/stdc++.h>
#define LOCAL
using namespace std;

namespace STDquantum {

const int N = 10e5 + 10;
int n, tot, cnt, ans, head[N], val[N];
struct E {
  int next, to, w;
} e[N << 1];
struct Trie {
  int vis[N][2];
  inline void add(int w) {
    int now = 0;
    for (int i = 30, d; ~i; --i) {
      d = (w >> i) & 1;
      if (!vis[now][d]) vis[now][d] = ++cnt;
      now = vis[now][d];
    }
  }
  inline int query(int w) {
    int ans = 0, now = 0;
    for (int i = 30, d; ~i; --i) {
      d = (w >> i) & 1;
      if (vis[now][d ^ 1]) {
        ans = (ans << 1) | (d ^ 1);
        now = vis[now][d ^ 1];
      } else {
        ans = (ans << 1) | d;
        now = vis[now][d];
      }
    }
    return ans;
  }
} T;

inline void add(int x, int y, int z) {
  e[++tot] = (E){head[x], y, z};
  head[x] = tot;
}
void dfs(int s, int fa) {
  for (int i = head[s]; i; i = e[i].next) {
    if (e[i].to != fa) {
      val[e[i].to] = val[s] ^ e[i].w;
      dfs(e[i].to, s);
    }
  }
}
inline void main() {
  scanf("%d", &n);
  for (int i = 1, x, y, z; i < n; ++i) {
    scanf("%d%d%d", &x, &y, &z);
    add(x, y, z), add(y, x, z);
  }
  dfs(1, 0);
  for (int i = 1; i <= n; ++i) { T.add(val[i]); }
  for (int i = 1; i <= n; ++i) {
    ans = max(ans, T.query(val[i]) ^ val[i]);
  }
  printf("%d", ans);
}

}  // namespace STDquantum

int main() {
#ifndef ONLINE_JUDGE
#ifdef LOCAL
  freopen("test.in", "r", stdin);
  freopen("test.out", "w", stdout);
#endif
#ifndef LOCAL
  freopen("P4551_Longest_Xor_Path.in", "r", stdin);
  freopen("P4551_Longest_Xor_Path.out", "w", stdout);
#endif
#endif
  STDquantum::main();
  return 0;
}
```

# CF241B Friends

链接：[CodeForces 241B](https://codeforces.com/problemset/problem/241/B) or [CF241B Friends](https://www.luogu.com.cn/problemnew/show/CF241B)

## 题意翻译

给定n个整数$a1,a2...an$,求两两异或值前k大的和。 其中$n<=50000,ai<=10^9$。答案对$1000000007 (10^9+7)$取模。

## 题目描述

You have $ n $ friends and you want to take $ m $ pictures of them. Exactly two of your friends should appear in each picture and no two pictures should contain the same pair of your friends. So if you have $ n=3 $ friends you can take $ 3 $ different pictures, each containing a pair of your friends. Each of your friends has an attractiveness level which is specified by the integer number $ a_{i} $ for the $ i $ -th friend. You know that the attractiveness of a picture containing the $ i $ -th and the $ j $ -th friends is equal to the exclusive-or ( $ xor $ operation) of integers $ a_{i} $ and $ a_{j} $ . You want to take pictures in a way that the total sum of attractiveness of your pictures is maximized. You have to calculate this value. Since the result may not fit in a 32-bit integer number, print it modulo $ 1000000007 $ $ (10^{9}+7) $ .

## 输入输出格式

### 输入格式

The first line of input contains two integers $ n $ and $ m $ $(1\le n\le5\cdot10^4;\ 0\le m\le\frac{n\cdot(n-1)}2)$ — the number of friends and the number of pictures that you want to take. Next line contains $ n $ space-separated integers $$ a_{1},a_{2},...,a_{n} $$ ( $$ 0<=a_{i}<=10^{9} $$ ) — the values of attractiveness of the friends.

### 输出格式

The only line of output should contain an integer — the optimal total sum of attractiveness of your pictures.

## 输入输出样例

### 输入样例 #1

```
3 1
1 2 3
```

### 输出样例 #1

```
3
```

### 输入样例 #2

```
3 2
1 2 3
```

### 输出样例 #2

```
5
```

### 输入样例 #3

```
3 3
1 2 3
```

### 输出样例 #3

```
6
```

# 题解

这题是 [P5283 [十二省联考2019]异或粽子](https://www.luogu.com.cn/problemnew/show/P5283) 的加强版。

带着代码讲吧，感谢`jiazhaopeng`神仙，题解代码我是真看不下去。

首先，我们先看最大的异或和是什么。

一个数在一位上是$0$，另一个数这一位是$1$，那么异或出来就会为$1$，也就保证了这一位对“使异或和最大”能做出的贡献最大。
