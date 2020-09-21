---
title: CF1400C Binary String Reconstruction
tags:
  - OI
  - 杂题
categories:
  - [OI, 字符串]
mathjax: true
date: 2020-08-27 15:52:52
---

一场Edu的C题

<!--more-->

# Binary String Reconstruction

## 题目描述

Consider the following process. You have a binary string (a string where each character is either 0 or 1) $ w $ of length $ n $ and an integer $ x $ . You build a new binary string $ s $ consisting of $ n $ characters. The $ i $ -th character of $ s $ is chosen as follows: 

- if the character $ w_{i-x} $ exists and is equal to 1, then $ s_i $ is 1 (formally, if $ i > x $ and $ w_{i-x} = $ 1, then $ s_i = $ 1);
- if the character $ w_{i+x} $ exists and is equal to 1, then $ s_i $ is 1 (formally, if $ i + x \le n $ and $ w_{i+x} = $ 1, then $ s_i = $ 1);
- if both of the aforementioned conditions are false, then $ s_i $ is 0. 

You are given the integer $ x $ and the resulting string $ s $ . Reconstruct the original string $ w $ .

## 输入输出格式

### 输入格式

The first line contains one integer $ t $ ( $ 1 \le t \le 1000 $ ) — the number of test cases. 

Each test case consists of two lines. The first line contains the resulting string $ s $ ( $ 2 \le |s| \le 10^5 $ , each character of $ s $ is either 0 or 1). The second line contains one integer $ x $ ( $ 1 \le x \le |s| - 1 $ ). 

The total length of all strings $ s $ in the input does not exceed $ 10^5 $ .

### 输出格式

For each test case, print the answer on a separate line as follows:

- if no string $ w $ can produce the string $ s $ at the end of the process, print $ -1 $ ; 
- otherwise, print the binary string $ w $ consisting of $ |s| $ characters. If there are multiple answers, print any of them.

## 输入输出样例

### 输入样例 #1

```
3
101110
2
01
1
110
1
```

### 输出样例 #1

```
111011
10
-1
```

## 题意翻译

给出一种01串的变换方法及对一个串变换后的结果串，求出原串（可以有多种），多组询问。

变换方法如下：

原串为 $w$，变换后的串为 $s$，两串长度相等；给出一个整数 $x$；01串从下标 $1$ 开始。

- 如果原串 $w_{i-x}$ 这一位存在且为 $\mathbf{1}$，那么 $s_i$ 为 $\mathbf1$（形式化地说，如果 $i\gt x$ 且 $w_{i-x}=\mathbf1$，那么 $s_i=\mathbf1$）。

- 如果原串 $w_{i+x}$ 这一位存在且为 $\mathbf{1}$，那么 $s_i$ 为 $\mathbf1$（形式化地说，如果 $i+ x\le n$ 且 $w_{i+x}=\mathbf1$，那么 $s_i=\mathbf1$）。

- 如果上述两种情况均不成立，那么 $s_i=\mathbf0$。

给出 $s$，求出一种可能的 $w$，若不存在任意一种 $w$，输出`-1`。

保证 $s$ 的总长度不超过 $10^5$，$t$（数据组数）不超过 $1000$。

## 题解

观察变换方式，可以发现在 $s_i=\mathbf0$ 的情况下，对原串有 $w_{i+x}=w_{i-x}=\mathbf0$（若存在的话）。

所以可以先扫一遍 $s$，标记这些位置，再处理 $s_i=\mathbf1$ 的情况。

```cpp
for (int i = 1; i <= len; ++i) {
    if (ch[i] == '0') {
        if (i - x > 0) vis[i - x] = 1;
        if (i + x <= len) vis[i + x] = 1;
    }
}
```

对于 $s_i=\mathbf1$ 的情况，可能有 $w_{i-x}=\mathbf1$ 或 $w_{i+x}=\mathbf1$ 或两者都有。而只要有一个就足够了，所以若两边都被打上了标记，就直接判断无解。可以只标记左边的 $w_{i-x}=1$，若 $w_{i-x}$ 已经被打上了标记，那就标记右边的 $w_{i+x}=1$。

有些位置可能不会被以上的方法赋值，这些位置是什么都可以，赋为`'0'`在第一个步骤比较方便。

思路很简单但是`if`太多而且每行很短，所以为了观感将缩进改为两格。

代码中`ch`串是结果串，`s`串是原串。

代码中还采用了C++的一个小trick：`goto`。这可以代替打标记+`break`，在情况较复杂时使用很方便。简单来说它的用法，那就是`goto wherever;`，若程序执行到这一句就会跑到`wherever:`的后面。可以用来跳出多个循环直接输出无解（或者类似的操作）。

```cpp
#include <bits/stdc++.h>
using namespace std;

namespace STDquantum { // 防止命名冲突用的，main()就相当于主函数

const int N = 3e5 + 10;
int t, x, len;
char ch[N], s[N];
bool vis[N];
inline void main() {
  scanf("%d", &t);
  while (t--) {
    scanf("%s%d", ch + 1, &x);
    len = strlen(ch + 1);
    fill(vis + 1, vis + len + 1, 0); // 可以代替memset把[vis+1,vis+len+1)全清零
    fill(s + 1, s + len + 1, '0'); // 清空s串
    s[len + 1] = 0; // printf遇见s[i]=0就会停止，所以这句相当于一个终止符
    for (int i = 1; i <= len; ++i) {
      if (ch[i] == '0') { // 第一个步骤：标记已有信息
        if (i - x > 0) vis[i - x] = 1;
        if (i + x <= len) vis[i + x] = 1;
      }
    }
    for (int i = 1; i <= len; ++i) {
      if (ch[i] == '1') { // 第二个步骤：求出原串
        if (i - x > 0) { // 若左边位置合法
          if (vis[i - x]) { // 若左边位置被标记过
            if (i + x <= len) { // 若右边位置合法
              if (vis[i + x]) { // 若右边位置被标记过
                goto school; // 都被标记过则无解
              } else {
                s[i + x] = '1'; // 左边被标记过、右边合法，就把右边赋为'1'
              }
            } else { // 右边位置不合法
              goto school; // 左边位置合法但被标记过，右边位置不合法，无解
            }
          } else { // 左边合法且没有被标记过直接赋为'1'
            s[i - x] = '1';
          }
        } else if (i + x <= len) { // 左边位置不合法，右边位置合法
          if (vis[i + x]) { // 右边被标记过
            goto school; // 左边位置不合法，右边位置合法但被标记过，无解
          } else {
            s[i + x] = '1'; // 左边位置不合法，右边位置合法且未被标记过，直接赋为'1'
          }
        } else {
          goto school; // 左右位置都不合法，无解
        }
      }
    }
    printf("%s\n", s + 1); // 全都合法直接输出原串
    continue;
  school: // goto school跳到这里，输出-1后直接进行下一次循环
    puts("-1");
  }
}

}  // namespace STDquantum

int main() {
  STDquantum::main();
  return 0;
}
```

# UPD on 2020-08-27 22:17:23

题解过了！翻译也是我的！

{% asset_image 1.png %}{% asset_image 2.png %}

