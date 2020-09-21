---
title: CF1406A Subset Mex
tags:
  - CF
categories:
  - [OI, 贪心]
mathjax: true
date: 2020-09-13 08:35:09
---

CF670 div2第一题

<!--more-->

空集的`\varnothing`锅了呜呜呜T_T

# Subset Mex

## 题目描述

Given a set of integers (it can contain equal elements). You have to split it into two subsets $ A $ and $ B $ (both of them can contain equal elements or be empty).

You have to maximize the value of $ mex(A)+mex(B) $ .

Here $ mex $ of a set denotes the smallest non-negative integer that doesn't exist in the set. For example: 

- $ mex(\{1,4,0,2,2,1\})=3 $ 
- $ mex(\{3,3,2,1,3,0,0\})=4 $
- $ mex(\emptyset)=0 $ ( $ mex $ for empty set) 

The set is splitted into two subsets $ A $ and $ B $ if for any integer number $ x $ the number of occurrences of $ x $ into this set is equal to the sum of the number of occurrences of $ x $ into $ A $ and the number of occurrences of $ x $ into $ B $ .

## 输入输出格式

### 输入格式

The input consists of multiple test cases. The first line contains an integer $ t $ ( $ 1\leq t\leq 100 $ ) — the number of test cases. The description of the test cases follows.

The first line of each test case contains an integer $ n $ ( $ 1\leq n\leq 100 $ ) — the size of the set.

The second line of each testcase contains $ n $ integers $ a_1,a_2,\dots a_n $ ( $ 0\leq a_i\leq 100 $ ) — the numbers in the set.

### 输出格式

For each test case, print the maximum value of $ mex(A)+mex(B) $ .

## 输入输出样例

### 输入样例 #1

```
4
6
0 2 1 5 0 1
3
0 1 2
4
0 2 0 1
6
1 2 3 4 5 6
```

### 输出样例 #1

```
5
3
4
0
```

## 说明

In the first test case, $ A=\left\{0,1,2\right\},B=\left\{0,1,5\right\} $ is a possible choice.

In the second test case, $ A=\left\{0,1,2\right\},B=\emptyset $ is a possible choice.

In the third test case, $ A=\left\{0,1,2\right\},B=\left\{0\right\} $ is a possible choice.

In the fourth test case, $ A=\left\{1,3,5\right\},B=\left\{2,4,6\right\} $ is a possible choice.

## 翻译

定义 $mex(S)$ 函数为不在 $S$ 集合中的最小自然数，如 $ mex(\{1,4,0,2,2,1\})=3 $。

给出一个集合，将其分为恰好两个子集（可为空集）$A$ 和 $B$，最大化 $mex(A)+mex(B)$。

多组数据。

## 题解

观察题意，发现我们要尽可能让两个子集的最小值平均分配。也就是如下规则：

1. 如果一个元素在原集合里出现了一次以上，那么两个子集都要包含这个元素。
2. 只出现了一次，就要都放在同一个集合里。

简易的证明：

拿样例举例

```
0 2 1 5 0 1
```

下表中是最优的分配办法（$\surd$ 表示集合中有该元素）：

|      |    A    |    B    |
| :--: | :-----: | :-----: |
|  5   | $\surd$ |         |
|  2   |         | $\surd$ |
|  1   | $\surd$ | $\surd$ |
|  0   | $\surd$ | $\surd$ |

这样分配为什么是最优的呢？

我可以拿掉 $B$ 中的 $0$，这样就违背了1，现在的两个集合变成了这样：

|      |    A    |    B    |
| :--: | :-----: | :-----: |
|  5   | $\surd$ |         |
|  2   |         | $\surd$ |
|  1   | $\surd$ | $\surd$ |
|  0   | $\surd$ |         |

可以发现 $mex(B)$ 变成了 $0$！

所以，让 $B$ 的 $mex$ 尽可能大就要把最小的那几个空尽可能填，如果填不上的话那就没有办法了，也不会再有更优的解了。

那么规则2是为什么呢？

还是举例：

```\
0 2 0 1
```

这时候遵守规则1和2，来填一下：

|      |    A    |    B    |
| :--: | :-----: | :-----: |
|  2   | $\surd$ |         |
|  1   | $\surd$ |         |
|  0   | $\surd$ | $\surd$ |

可以发现所有出现了一次的元素都跑到了 $A$ 里面。

那么若不遵守规则2呢？

|      |    A    |    B    |
| :--: | :-----: | :-----: |
|  2   |         | $\surd$ |
|  1   | $\surd$ |         |
|  0   | $\surd$ | $\surd$ |

可以看到 $mex(A)=2,mex(B)=1$，然而最优的 $mex$ 和是 $4$。填到 $B$ 里面的那个 $2$ 不但没有作用，而且还让 $mex(A)$ 减小了。

所以如果没法平均分配就都贪心地放到一边，让这一边的更大，才可能得到更优的解。

综上，我们要遵守这两条规则，就能得到最优解了。

代码（效率比较低但是符合上述思考过程）：

```cpp
const int N = 200;
int t, n, a[N], vis[N]; // vis表示集合中元素出现个数
bool is[N], isA[N], isB[N]; // is用来标记元素是否处理过，在填集合的时候用
int main() {
    scanf("%d", &t);
    while (t--) {
        scanf("%d", &n);
        memset(vis, 0, sizeof(vis)), memset(is, 0, sizeof(is));
        memset(isA, 0, sizeof(isA)), memset(isB, 0, sizeof(isB));
        for (int i = 1; i <= n; ++i) { scanf("%d", a + i), ++vis[a[i]]; }
        sort(a + 1, a + n + 1); // 排序后更易处理
        int ans = 0;
        vector<int> A, B;
        for (int i = 1; i <= n; ++i) {
            if (is[a[i]]) continue;
            if (vis[a[i]] > 1) {
                /* 如果出现次数大于1，符合规则1 */
                A.push_back(a[i]), B.push_back(a[i]), is[a[i]] = 1;
                isA[a[i]] = isB[a[i]] = 1;
            } else {
                /* 只出现一次，符合规则2 */
                A.push_back(a[i]), isA[a[i]] = 1;
            }
        }
        
        /* 求mex的操作：从小往大遍历，如果发现第一个不在集合中的自然数就停止。 */
        for (int i = 0; i <= 100; ++i) {
            if (!isA[i]) {
                ans += i;
                break;
            }
        }
        for (int i = 0; i <= 100; ++i) {
            if (!isB[i]) {
                ans += i;
                break;
            }
        }
        printf("%d\n", ans);
    }
    return 0;
}
```

注：其中的vector只是为了表明集合中有这些元素，实际上只需要记录`isA`和`isB`数组就够了。

# UPD on 20200914

09-13 21:05题解通过了