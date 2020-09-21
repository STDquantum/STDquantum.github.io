---
title: UVA1521 GCD Guessing Game
tags:
  - OI
categories:
  - [OI, 数学]
mathjax: true
date: 2020-08-23 17:11:07
---

与质数和 $\gcd$ 有关的题。

<!--more-->

# GCD Guessing Game

[UVA1521 GCD Guessing Game](https://www.luogu.com.cn/problem/UVA1521)

## 题意翻译

多组数据 

给定一个正整数n

要求猜一个（1～n）之间的数字p 

每次可以猜一个数，不妨叫x 

然后呢会告诉你x和p的最大公约数 

要求使用最优策略，使最坏情况下猜数次数最少 

输出最少次数 

比如n=6的时候可以直接猜6，若回答1，则p为1或5，再猜一次就好了，为2，就是2或4，为3就是3，为6就是6 

所以最坏情况是2

## 题目描述

[problemUrl]( https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=448&page=show_problem&problem=4267 ) [PDF](https://uva.onlinejudge.org/external/15/p1521.pdf)

## 输入输出格式

### 输入格式

一行一个 $n$，多组数据。

### 输出格式

对每个 $n$ 输出最优策略下最坏猜几次。 

## 输入输出样例

### 输入样例 #1

```
6
```

### 输出样例 #1

```
2
```

## 题解

这是一篇解释“为什么想到要选质数”以及“为什么选质数是对的”（也就是其他大佬面临的问题）的题解。

如果不太懂欢迎提问qwq，如果我说的有哪错了欢迎提出。

正解不是我想出来的，感谢[shame_djj](https://www.luogu.com.cn/user/162867)和[naive_wcx](https://www.luogu.com.cn/user/23138)大佬。

做这道题的时候思路可以是这样的，先考虑非最优的策略，再去想怎么优化它。

所以一开始想的时候，就是先从最大值（也就是 $n$）向下枚举。如果出现了它给出的数字（以下简称 $g$，代表 $\gcd$）和猜想的数字（以下简称 $x$）完全相同，也就是说明原数字（也就是答案数字（不是要求的那个答案），以下简称 $ans$）是 $x$ 的倍数，但是 $ans$ 又不可能比 $x$ 大，因为是一路枚举下来的，所以当 $g=x$ 就结束了。

最坏是 $n-1$ 次，对应的 $ans$ 是 $1$。这样的话会多出许多冗余步骤，那些步骤对求出 $ans$ 没有一点帮助。比如在 $n=12,ans=1$ 的情况下枚举完了 $12$，又去枚举一遍 $6$，对求解规模的缩小没有一点帮助（除了减了一）。

我们先对排除做一个定义吧，就是：猜想一个 $x=p_1^{k_1}p_2^{k_2}\dots p_m^{k_m}$，对方给出的 $g$ 中不包含某些因子，如 $p_1$，这就说明 $ans$ 中是没有 $p_1$ 这个因子的，否则的话 $p_1$ 一定能整除 $g$。这样我们就可以排除所有含 $p_1$ 这个因子的 $ans$，原本是 $n$ 的求解规模减少了 $\displaystyle\left\lfloor\frac n{p_1}\right\rfloor$（肯定比枚举这些被排除的数要优吧）。

当然我们可以发现最难找的肯定就是 $ans=1$，因为要排除所有其他的数才能让 $1$ 当做答案，而暴力枚举排除所有数显然是不行的。

至于证明，可以假设找到 $1$ 的步数不是最优方案下的最坏步数，那么肯定可以找到一个大于 $1$ 的数（设为 $p_0$），让它必须要花更多的步数才能被找到。想要找到这个数，肯定存在一步使我们能排除 $1$ 而不能排除 $p_0$，可是 $1$ 无法通过 $g$ 的值进行排除，给出 $g=1$ 都无法确定到底哪个是答案，反而 $p_0$ 大于 $1$，是可以通过给出的 $g$ 值进行排除的。所以一定不存在一个数需要排除的步数比 $1$ 大，这个问题也就转化为了**最少花多少步可以筛掉所有大于** $1$ **的数**。

那么，我们就是要找一个最优的方案让我们非常快速地把数筛掉，那么显然对于 $x=p_1^{k_1}p_2^{k_2}\dots p_m^{k_m}$，所有的 $k$ 都等于 $1$ 就好，因为筛掉数字只和质因子有关，和到底有多少没关系（所以对于前文 $n=12$ 的例子，$x=12$ 和 $x=6$ 对最坏答案的求解规模减小量的贡献是一样的，因为它们两个都只有 $2,3$ 两个质因子，所以也就没有必要把这两个数都枚举一遍）。

而 $n$ 内所有大于 $1$ 的数，是可以被所有 $[2,n]$ 内的质数筛掉的，所以现在问题就变成了**怎么安排最少的** $n$ **以内的数字使得** $[2,n]$ **中的质数作为质因子尽快全部出现**。（这句可能有点绕哈见谅）

接下来就是贪心的事了，用质数来筛是最优的（详情见筛质数的算法，在筛出质数的同时筛了所有其他数），所以我们要把 $[2,n]$ 内所有质数以乘为运算符全塞进小于等于 $n$ 的数中，找一个最大的搭配一堆最小的就行。双指针维护一下（或者随便什么方法能贪心就行）。

这样每次是 $O(p)$ 的（$p$ 为 $[1,n]$ 中质数的数量），总共 $O(Tp)$，可以通过本题（虽然没给 $T$ 但肯定是 $O($能过$)$）。

```cpp
#include <bits/stdc++.h>
using namespace std;

namespace STDquantum {

const int N = 1e4 + 10;
int num, prime[N];
bool isPrime[N];
inline void Get(int n) { // 线性筛质数
    memset(isPrime, 1, sizeof(isPrime));
    isPrime[1] = 0;
    for (int i = 2; i <= n; ++i) {
        if (isPrime[i]) prime[++num] = i;
        for (int j = 1; j <= num && i * prime[j] <= n; ++j) {
            isPrime[i * prime[j]] = 0;
            if (i % prime[j] == 0) break;
        }
    }
}

int n, ans;
inline void main() {
    Get(N - 10);
    while (~scanf("%d", &n) && n) {
        ans = 0;
        int l = 1, r = num;
    	while (prime[r] > n) --r; // 让prime[r]降到[2,n]的范围
        for (int k; l <= r; --r, ++ans) {
            k = prime[r];
            while (k * prime[l] <= n) k *= prime[l++];
        }
        printf("%d\n", ans);
    }
}

}   // namespace STDquantum

int main() {
    STDquantum::main();
    return 0;
}
```

悄咪咪说一句，POJ上没有题面的4028和这题一毛一样……