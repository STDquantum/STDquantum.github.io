---
title: 'POJ3481 Double Queue'
date: 2019-12-23 18:59:00
tags: 
- STL
- 优先队列
mathjax: true
categories:
  - [OI, 优先队列]
---



**用到了一些有趣的方法**

<!--more-->

## Double Queue

#### Description

The new founded Balkan Investment Group Bank (BIG-Bank) opened a new office in Bucharest, equipped with a modern computing environment provided by IBM Romania, and using modern information technologies. As usual, each client of the bank is identified by a positive integer $K$ and, upon arriving to the bank for some services, he or she receives a positive integer priority $P$. One of the inventions of the young managers of the bank shocked the software engineer of the serving system. They proposed to break the tradition by sometimes calling the serving desk with the lowest priority instead of that with the highest priority. Thus, the system will receive the following types of request:

---------------------------------------------

|              |                                                              |
| :----------: | :----------------------------------------------------------: |
|     $0$      |               The system needs to stop serving               |
| $1$  $K$ $P$ |     Add client $K$ to the waiting list with priority $P$     |
|     $2$      | Serve the client with the highest priority and drop him or her from the waiting list |
|     $3$      | Serve the client with the lowest priority and drop him or her from the waiting list |

-------------------

Your task is to help the software engineer of the bank by writing a program to implement the requested serving policy.

#### Input

Each line of the input contains one of the possible requests; only the last line contains the stop-request (code 0). You may assume that when there is a request to include a new client in the list (code 1), there is no other request in the list of the same client or with the same priority. An identifier $K$ is always less than 10^6^, and a priority $P$ is less than 10^7^. The client may arrive for being served multiple times, and each time may obtain a different priority.

#### Output

For each request with code 2 or 3, the program has to print, in a separate line of the standard output, the identifier of the served client. If the request arrives when the waiting list is empty, then the program prints zero (0) to the output.

#### Sample Input

2
1 20 14
1 30 3
2
1 10 99
3
2
2
0

#### Sample Output

0
20
30
10
0 

## 解题

这道题需要用的是堆或者优先队列（本篇以优先队列为例），而且需要保证优先队列能够进行非顶部的删除操作。

##### 例：

分别将1,2,3放进从大到小的队列$P$和从小到大的队列$Q$
$P$ : 3 2 1
$Q$ : 1 2 3
若要取出$Q$队列的顶部，并将$P$中对应元素删除（或刚好相反），只用两个队列是无法完成这种操作的，因此需要引入两个辅助优先队列$A$（从大到小）、$B$（从小到大）。且每次在需要删除时将删除元素放入与被删除队列优先相反的辅助队列（如$P$->$B$）里，每次需要提取$top$的时候若原队列的$top$与优先对应的辅助队列的$top$相同，则同时$pop$，否则取出原队列的$top$。

##### End of 例

好啦就是这种思想，所以看看代码吧。。。。

```cpp
#include<cstdlib>
#include<cstdio>
#include<iostream>
#include<cstring>
#include<algorithm>
#include<queue>//优先队列所在头文件
using namespace std;
int x,pe,cx;
bool f;
pair<int,int> a;//用pair能解决两个一块的元素的问题
//带个e的与原队列优先对应的辅助队列
priority_queue<pair<int,int> >hi,hie;
priority_queue<pair<int,int>,vector<pair<int,int> >,greater<pair<int,int> > >lo,loe;
int main(){
	while(1){
		scanf("%d",&x);
		switch(x){
			case 1:
				scanf("%d%d",&pe,&cx);
				a=make_pair(cx,pe);
				lo.push(a);hi.push(a);
				break;
			case 2:
				while(!hi.empty()&&!hie.empty()){
					if(hie.top()==hi.top())hie.pop(),hi.pop();
					else break;
				}
				if(hi.empty())printf("0\n");
				else printf("%d\n",hi.top().second),loe.push(hi.top()),hi.pop();
				break;
			case 3:
				while(!lo.empty()&&!loe.empty()){
					if(loe.top()==lo.top())loe.pop(),lo.pop();
					else break;
				}
				if(lo.empty())printf("0\n");
				else printf("%d\n",lo.top().second),hie.push(lo.top()),lo.pop();
				break;
			default:f=1;break;
		}
		if(f)break;
	}
	return 0;
}

```

所以这道题就这么解决了
{% asset_img 1.png POJ上交的评测记录 %}

$\LARGE 2019.12.23$