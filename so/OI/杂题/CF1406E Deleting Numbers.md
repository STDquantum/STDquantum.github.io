---
title: CF1406E Deleting Numbers
tags:
  - CF
  - 数论
  - 交互
categories:
  - [OI, 交互题]
mathjax: true
date: 2020-09-13 09:33:57
---

CF670 Div2的E题，做的第一道交互。

<!--more-->

[CF1406E Deleting Numbers](https://www.luogu.com.cn/problem/CF1406E)

# Deleting Numbers

## 题目描述

This is an interactive problem.

There is an unknown integer $ x $ ( $ 1\le x\le n $ ). You want to find $ x $ .

At first, you have a set of integers $ \{1, 2, \ldots, n\} $ . You can perform the following operations no more than $ 10000 $ times: 

- A $ a $ : find how many numbers are multiples of $ a $ in the current set.
- B $ a $ : find how many numbers are multiples of $ a $ in this set, and then delete all multiples of $ a $ , but $ x $ will never be deleted (even if it is a multiple of $ a $ ). In this operation, $ a $ must be greater than $ 1 $ . 
- C $ a $ : it means that you know that $ x=a $ . This operation can be only performed once.

Remember that in the operation of type B $ a>1 $ must hold.

Write a program, that will find the value of $ x $ .

## 输入输出格式

### 输入格式

The first line contains one integer $ n $ ( $ 1\le n\le 10^5 $ ).

The remaining parts of the input will be given throughout the interaction process.

### 输出格式

In each round, your program needs to print a line containing one uppercase letter A, B or C and an integer $ a $ ( $ 1\le a\le n $ for operations A and C, $ 2\le a\le n $ for operation B). This line desribes operation you make.

If your operation has type C your program should terminate immediately.

Else your program should read one line containing a single integer, which is the answer to your operation.

After outputting each line, don't forget to flush the output.

To do it use: 

- fflush(stdout) in C/C++;

- System.out.flush() in Java; 
- sys.stdout.flush() in Python;
- flush(output) in Pascal; 
- See the documentation for other languages.

It is guaranteed, that the number $ x $ is fixed and won't change during the interaction process. 

Hacks:

To make a hack, use such input format:

The only line should contain two integers $ n $ , $ x $ ( $ 1 \leq x \leq n \leq 10^5 $ ).

## 输入输出样例

### 输入样例 #1

```
10

2

4

0
```

### 输出样例 #1

```
B 4

A 2

A 8

C 4
```

## 说明

Note that to make the sample more clear, we added extra empty lines. You shouldn't print any extra empty lines during the interaction process.

In the first test $ n=10 $ and $ x=4 $ .

Initially the set is: $ \{1,2,3,4,5,6,7,8,9,10\} $ .

In the first operation, you ask how many numbers are multiples of $ 4 $ and delete them. The answer is $ 2 $ because there are two numbers divisible by $ 4 $ : $ \{4,8\} $ . $ 8 $ will be deleted but $ 4 $ won't, because the number $ x $ will never be deleted. Now the set is $ \{1,2,3,4,5,6,7,9,10\} $ .

In the second operation, you ask how many numbers are multiples of $ 2 $ . The answer is $ 4 $ because there are four numbers divisible by $ 2 $ : $ \{2,4,6,10\} $ .

In the third operation, you ask how many numbers are multiples of $ 8 $ . The answer is $ 0 $ because there isn't any number divisible by $ 8 $ in the current set.

In the fourth operation, you know that $ x=4 $ , which is the right answer.

## 翻译

### 题目描述

这是一道交互题。

有一个未知整数 $x$（$ 1\le x\le n $），你需要找到它。

一开始，你有一个整数集合 $ \{1, 2, \ldots, n\} $，你可以对其进行如下三种操作，操作总数（包括 A, B, C）不超过 $10000$ 次。

- A $a$：询问当前集合中有多少 $a$ 的倍数。
- B $a$：询问当前集合中有多少 $a$ 的倍数，并删掉所有 $a$ 的倍数（$x$ 不会被当前操作删去）。此操作中的 $a$ 应大于 $1$。

- C $a$：进行此操作意味着你知道了 $x=a$，此操作只能执行一次。

### 输入格式

第一行包括一个正整数 $n$。

其余部分的输入将在交互过程中给出。

### 输出格式

在每个回合中，你的程序需要输出一行，包括一个大写字母 A, B 或 C，和一个整数 $a$（对操作 A 和 C 应满足 $ 1\le a\le n $，对操作 B 应满足 $ 2\le a\le n $），表示你做的操作。

如果为操作 C，你的程序应当立即结束。

否则你应该读入一行，包括一个正整数，表示你查询的结果。

在输出每一行后，别忘了刷新输出流。

- C++ 中使用 `fflush(stdout)`
- Java 中使用 `System.out.flush()`

- Python 中使用 `sys.stdout.flush()`
- Pascal 中使用 `flush(output)`

保证 $x$ 在交互过程中不会改变。

### 提示

为了使样例更清晰，加入了多余的空行。你的程序不应在交互过程中输出多余空行。

### 样例解释

对当前样例有 $n=10,x=4$。

最初的集合是：$ \{1,2,3,4,5,6,7,8,9,10\} $。

在第一次操作中，你询问有多少数是 $4$ 的倍数并删去这些数。答案是 $2$，因为有两个数能被 $4$ 整除：$\{4,8\}$。$8$ 会被删除但是 $4$ 不会，因为 $x$ 永远不会被删掉。现在集合变成了 $ \{1,2,3,4,5,6,7,9,10\} $。

在第二次操作中，你询问有多少数是 $2$ 的倍数。答案是 $4$，因为有四个数能被 $2$ 整除：$ \{2,4,6,10\} $。

在第三次操作中，你询问有多少个数是 $8$ 的倍数，答案是 $0$，因为当前集合没有数能被 $8$ 整除。

在第四次操作中，你知道了 $x=4$，为正确答案。

## 题解

这题一个很直接的思路就是直接拿质数来筛出 $x$，如果把 $x$ 的所有质因子和对应指数找到，那么就可以确定 $x$。

可以证明这么做平均是最快的。

那么具体怎么做呢？我们考虑把一个质数 $p$ 的所有倍数删去，若有一个数没有被删除成功，那就说明 $x$ 中有 $p$ 这个因子。

一种朴素的做法是直接删掉所有 $p$ 的倍数，然后查询 $p$ 的倍数有几个，如果是 $1$ 的话就说明有 $p$ 这个因子，然后接着二分 $p$ 的指数。

但是这样的操作次数是不对的，因为 $100000$ 以内的质数就有 $9592$ 个，一删一查次数就多了，所以考虑怎么优化这个暴力。

删质数的这一次是肯定要留着的，但是查询有大量的冗余操作，所以可以考虑分批查询。

这就有一些分块的思想了。

为了让复杂度尽可能稍微均摊一点，设质数的个数为 $num$，则分成 $\sqrt {num}$ 个块，先全部删掉所有块内的质数，然后查询一个`A 1`，就可以查看之前一共删了多少个数字。

如果这个数字和之前删质数时候查询个数的和一样，就说明 $x$ 中是没有这个块内的质因子的。

如果不一样，说明有这个质因子，一个一个试查询就好了。若确定有这个质因子就寻找其指数，然而我二分容易写锅，就直接一个一个乘上去查。

可以结合代码理解：

```cpp
#include <bits/stdc++.h>
using namespace std;

namespace STDquantum {

typedef long long ll;
const int N = 2e6 + 10;
int num, prime[N];
bool isprime[N];
inline void Get(int n) { // 筛质数
    memset(isprime, 1, sizeof(isprime));
    isprime[1] = 0;
    for (int i = 2; i <= n; ++i) {
        if (isprime[i]) prime[++num] = i;
        for (int j = 1; j <= num && i * prime[j] <= n; ++j) {
            isprime[i * prime[j]] = 0;
            if (i % prime[j] == 0) break;
        }
    }
}

int n, x;
ll ans = 1; // ans代表现在找到的数

inline void query(int type, int value) { // 询问和读入
    if (type == 1) {
        printf("A %d\n", value);
    } else if (type == 2) {
        printf("B %d\n", value);
    } else {
        printf("C %d\n", value);
        fflush(stdout);
        exit(0);
    }
    fflush(stdout);
    scanf("%d", &x);
}

inline void Find(int p) { // 找到x中p的指数
    while (ans * p <= n) {
        query(1, ans * p);
        if (x) {
            ans *= p;
        } else {
            break;
        }
    }
}

inline void main() {
    scanf("%d", &n), Get(n);
    int s = sqrt(num); // 分成s个块
    for (int i = 1, cur = n, sum; i <= num; i += s) { // 枚举块的左端点
        sum = 0;
        for (int j = 0; i + j <= num && j < s; ++j) { // 枚举块内第几个点
            query(2, prime[i + j]), sum += x; // 查询并加和
        }
        query(1, 1);
        int tmp = x; // 临时记录一下现在集合内还剩多少数
        if (sum > cur - x) { // 如果这次删去的数的总数大于实际减少的总数，就说明质因子在此块内出现过
            for (int j = 0; i + j <= num && j < s; ++j) {
                Find(prime[i + j]);
            }
        }
        cur = tmp; // 更新一下集合里当前数字个数
    }
    query(3, ans); // 输出答案，因为ans初始化就是1所以没必要特判了。
}

}   // namespace STDquantum

int main() {
    STDquantum::main();
    return 0;
}
```

# UPD on 20200914

09-13 23:08题解通过了