---
title: CF1400A String Similarity
tags:
  - OI
  - 杂题
categories:
  - [OI, 字符串]
mathjax: true
date: 2020-08-27 15:15:49
---

一场Edu的A题

<!--more-->

# String Similarity

## 题目描述

A binary string is a string where each character is either 0 or 1. Two binary strings $ a $ and $ b $ of equal length are similar, if they have the same character in some position (there exists an integer $ i $ such that $ a_i = b_i $ ). 

For example: 

- 10010 and 01111 are similar (they have the same character in position $ 4 $ );
- 10010 and 11111 are similar;
- 111 and 111 are similar; 
- 0110 and 1001 are not similar.

You are given an integer $ n $ and a binary string $ s $ consisting of $ 2n-1 $ characters. Let's denote $ s[l..r] $ as the contiguous substring of $ s $ starting with $ l $ -th character and ending with $ r $ -th character (in other words, $ s[l..r] = s_l s_{l + 1} s_{l + 2} \dots s_r $ ). 

You have to construct a binary string $ w $ of length $ n $ which is similar to all of the following strings: $ s[1..n] $ , $ s[2..n+1] $ , $ s[3..n+2] $ , ..., $ s[n..2n-1] $ .

## 输入输出格式

### 输入格式

The first line contains a single integer $ t $ ( $ 1 \le t \le 1000 $ ) — the number of test cases. 

The first line of each test case contains a single integer $ n $ ( $ 1 \le n \le 50 $ ). 

The second line of each test case contains the binary string $ s $ of length $ 2n - 1 $ . Each character $ s_i $ is either 0 or 1.

### 输出格式

For each test case, print the corresponding binary string $ w $ of length $ n $ . If there are multiple such strings — print any of them. It can be shown that at least one string $ w $ meeting the constraints always exists.

## 输入输出样例

### 输入样例 #1

```
4
1
1
3
00000
4
1110000
2
101
```

### 输出样例 #1

```
1
000
1010
00
```

## 说明

The explanation of the sample case (equal characters in equal positions are bold): 

The first test case: 

- $ \mathbf{1} $ is similar to $ s[1..1] = \mathbf{1} $ . 

The second test case: 

- $ \mathbf{000} $ is similar to $ s[1..3] = \mathbf{000} $ ; 
- $ \mathbf{000} $ is similar to $ s[2..4] = \mathbf{000} $ ; 
- $ \mathbf{000} $ is similar to $ s[3..5] = \mathbf{000} $ . 

The third test case: 

- $ \mathbf{1}0\mathbf{10} $ is similar to $ s[1..4] = \mathbf{1}1\mathbf{10} $ ; 
- $ \mathbf{1}01\mathbf{0} $ is similar to $ s[2..5] = \mathbf{1}10\mathbf{0} $ ;
- $ \mathbf{10}1\mathbf{0} $ is similar to $ s[3..6] = \mathbf{10}0\mathbf{0} $ ;
- $ 1\mathbf{0}1\mathbf{0} $ is similar to $ s[4..7] = 0\mathbf{0}0\mathbf{0} $ . 

The fourth test case:

- $ 0\mathbf{0} $ is similar to $ s[1..2] = 1\mathbf{0} $ ; 
- $ \mathbf{0}0 $ is similar to $ s[2..3] = \mathbf{0}1 $ .

## 题意翻译

我们定义两个长度相等的01串是相似的，当且仅当两个01串中至少有一位是相同的。

给出一个长度为 $2n-1$ 的01串 $s$，构造出一个长度为 $n$ 的01串 $s^\prime$，使得对于 $s$ 的每一个长度为 $n$ 的子串 $ s[1..n] $ , $ s[2..n+1] $ , $ s[3..n+2] $ , ..., $ s[n..2n-1] $，都和 $s^\prime$ 相似。

输入有多组数据，第一行为 $n$，第二行是长度为 $2n-1$ 的01串 $s$。

对于每组输入，输出一行，为构造出的01串 $s^\prime$。

## 题解

可以发现，小串 $s^\prime$ 一共匹配了 $n$ 次，而 $s^\prime$ 长度刚好为 $n$。

也就是说，我们可以让每次匹配相似的时候都匹配 $s^\prime$ 的不同位（因为只需匹配一位，也就是其他位根本不用管，单独考虑就行）。

比如当 $n=5,s=\mathbf{101011010}$ 时，如下面形式匹配：

第一次匹配，不去考虑其他位，在 $s^\prime$ 第一位填上 $s$ 的第一位，这时已经可以保证 $s^\prime$ 和 $s$ 的第一个子串相似了。

| $s^\prime$ | 1    |      |      |      |      |      |      |      |      |
| ---------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| $s$        | 1    | 0    | 1    | 0    | 1    | 1    | 0    | 1    | 0    |

第二次匹配：这时 $s^\prime$ 的第一位已经没有用了（因为只需匹配一位），我们将它向右移动一位匹配第二个 $s$ 的子串，并将 $s^\prime$ 的第二位填上对应的 $s$ 的这一位。

| $s^\prime$ |      | 1    | 1    |      |      |      |      |      |      |
| ---------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| $s$        | 1    | 0    | 1    | 0    | 1    | 1    | 0    | 1    | 0    |

第三、四、五次匹配同理，向右移动一位，再填上一位。

| $s^\prime$ |      |      | 1    | 1    | 1    |      |      |      |      |
| ---------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| $s$        | 1    | 0    | 1    | 0    | 1    | 1    | 0    | 1    | 0    |

| $s^\prime$ |      |      |      | 1    | 1    | 1    | 0    |      |      |
| ---------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| $s$        | 1    | 0    | 1    | 0    | 1    | 1    | 0    | 1    | 0    |

| $s^\prime$ |      |      |      |      | 1    | 1    | 1    | 0    | 0    |
| ---------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| $s$        | 1    | 0    | 1    | 0    | 1    | 1    | 0    | 1    | 0    |

这时就已经移动到了最后， 我们也得到了 $s^\prime=\mathbf{11100}$。

可以检验一下，这么填出来的答案能保证和 $s$ 的每个长度为 $n$ 的子串相似。

这样就已经可以做出答案了，但是我们还可以做得更快。

可以发现每次填上对应位的时候，对应的那个 $s$ 上的位数一定是奇数（从 $1$ 开始计算），所以直接在读入时输出奇数位或者读入完输出奇数位即可。

主要部分（`ch`是 $s$ 串）：

```cpp
scanf("%d", &t);
for (int i = 1; i <= t; ++i) {
    scanf("%d%s", &n, ch + 1);
    for (int j = 1; j <= 2 * n - 1; ++j) {
        if (j % 2 == 1) cout << ch[j]; // 奇数位就输出
    }
    puts(""); // 换行
}
```

# UPD on 2020-08-27 22:20:56

翻译没用我的，但是题解过了。

{% asset_image 1.png %}