---
title: 'BZOJ1601 [Usaco2008 Oct] 灌水'
date: 2020-02-16 07:26:23
tags: 最小生成树
mathjax: true
categories:
  - [OI, 图论, 最小生成树]
---

题如其名，掌握了方法，这道题还是挺水的。<!-- more -->

# BZOJ1601 [Usaco2008 Oct] 灌水

## Description

Farmer John已经决定把水灌到他的$n$$(1\le n\le 300)$块农田，农田被数字$1$到$n$标记。把一块土地进行灌水有两种方法，从其他农田饮水，或者这块土地建造水库。 建造一个水库需要花费$w_i(1\le w_i\le 100000)$,连接两块土地需要花费$P_{i,j}(1\le p_{i,j}\le 100000,p_{i,j}=p_{j,i},p_{i,i}=0)$. 计算Farmer John所需的最少代价。

## Input

- 第一行：一个数$n$
- 第二行到第$n+1$行：第$i+1$行含有一个数$w_i$
- 第$n+2$行到第$2n+1$行：第$n+1+i$行有$n$个被空格分开的数，第$j$个数代表$p_{i,j}$。

## Output

- 第一行：一个单独的数代表最小代价.

## Sample Input

```
4
5
4
4
3
0 2 2 2
2 0 3 3
2 3 0 4
2 3 4 0
```

## Sample Output

```
9
```



输出详解：

Farmer John在第四块土地上建立水库，然后把其他的都连向那一个，这样就要花费$3+2+2+2=9$

## 解题

对于每块土地，我们发现必须要连接另一块土地或者在此建水库。

不妨建立一个$0$号节点，将$0$与每块土地连边，边权是在这块土地上建水库的费用。

举个栗子，就拿样例说话。

我们把图建出来大概是这个样子：

{% asset_image "BZOJ1601 [Usaco2008 Oct] 灌水.png" 用样例建出的图 %}

是不是特别有规律？是的。这就是张完全图（[附上度娘](https://baike.baidu.com/item/%E5%AE%8C%E5%85%A8%E5%9B%BE/10073908?fr=aladdin)），至于为什么，就是依照题意，每两个点都要连边。然后我们需要在这张图里建最小生成树，边权和就是需要的答案。

是不是又要问为什么了？

回顾下我们前面把与$0$连边视为建水库，其主要目的就是为了让整个图确保有水库，以及可以直接建水库而不受其他点干扰。

还是拿栗子说话。手模一下样例，并且结合样例解释，不难发现最后结果就是这个样子的（最小生成树由粗线描出）：

{% asset_image 1.png 样例最小生成树 %}它既保证了每块土地都能连接到水库或与水库连接的土地，又保证边权和最小。

由此进行合理外推，最小生成树即为最优情况。

所以剩下的就简单了，不要告诉我你不会最小生成树。不会也罢。

```c++
#include<bits/stdc++.h>
using namespace std;
const int N=500001;//节点数最大值+1
int n,parent[N],tot,cnt;
//分别为节点数n，子节点数或父节点parent，总边数tot，已选边数cnt
long long ans;//总权值的答案
struct M{int from,to,val;}e[N];//存边用，无向边两端点from和to，权值val
bool cmp(M a,M b){return a.val<b.val;}//结构体数组比较用，按权值小到大排序
void UFset(){for(int i=0;i<=n;++i)parent[i]=-1;}//并查集初始化，注意0是超级源点，要加上
int Find(int x){//并查集查找
	int s=x;
	for(;parent[s]>=0;s=parent[s]);//注意后面的分号和前面parent[s]>=0，还是因为有0号节点
	while(s!=x){//让查找次数变少的一种操作
		int tmp=parent[x];
		parent[x]=s;
		x=tmp;
	}
	return s;
}
void Union(int R1,int R2){//并查集合并操作
	int r1=Find(R1),r2=Find(R2);
	int tmp=parent[r1]+parent[r2];
	if(parent[r1]>parent[r2]){//如果r1子节点数小于r2子节点数
		parent[r1]=r2;//让r1的根节点变为r2
		parent[r2]=tmp;//让r2子节点数变为之前r1和r2子节点数之和
	}
	else{//与上面同理
		parent[r1]=tmp;
		parent[r2]=r1;
	}
}
void Kruskal(){//克鲁斯卡尔求最小生成树
	UFset();
	sort(e+1,e+tot+1,cmp);//边从小到大排序
	for(int i=1;i<=tot;++i){
		int u=e[i].from,v=e[i].to,w=e[i].val;
		if(Find(u)!=Find(v)){//如果不在一个集合里
			Union(u,v);//合并集合
			ans+=w,++cnt;//边权值加到答案里，选定边个数加1
		}
		if(cnt==n)break;//共有0和1~n这(n+1)个节点，最小生成树上有n条边
	}
	printf("%d",ans);
}
int main(){
	// freopen("1601.in","r",stdin);freopen("1601.out","w",stdout);
	scanf("%d",&n);
	for(int i=1;i<=n;++i){
		int w;
		scanf("%d",&w);
		e[++tot].from=0,e[tot].to=i,e[tot].val=w;//连接0号节点，原地建水库
	}
	for(int i=1;i<=n;++i){
		for(int j=1;j<=n;++j){
			int x;
			scanf("%d",&x);
			if(i<j){//一点优化，因为方阵是对称的而且自环没必要加上
				e[++tot].from=i,e[tot].to=j,e[tot].val=x;
			}
		}
	}
	Kruskal();
	return 0;//功德圆满
}
```

所以这道题就告一段落了。

$\huge {写于2020.02.16下午}$

