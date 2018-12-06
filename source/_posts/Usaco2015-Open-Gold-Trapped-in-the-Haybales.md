---
title: '[Usaco2015 Open Gold]Trapped in the Haybales'
date: 2017-06-03 12:16:24
tags:
---
# 题意：
农民约翰已经收到了一批有N个的大型干草包（1<=N<=100,000），并放置在沿着通往他的谷仓的路不同位置。不幸的是，他完全忘记了Bessie牛是放牧的路上，她现在可能被困在那些干草包中！每个干草包J都有一个大小S[J]和一个位置P[J]，提供那个干草包在那个一维道路的位置。贝茜奶牛可以在路上自由走动，甚至到这个干草包所在的位置，但她无法穿越这个位置。作为一个例外，如果她在同一个方向跑D个速度单位，她就可以有足够的速度突破，并永久消除任何干草包，只要那个干草包大小严格小于D。当然，在这之后，她会打开更大的空间让她跑向别的干草包，并消除它们。
如果她能突破最左边或最右边的干草包，则贝茜可以逃掉。请计算由实数的起始位置组成的道路最小需要多长，使Bessie不能逃脱。

<!--more-->
题目链接：https://www.luogu.org/problem/show?pid=3127

# 题解：
题目好绕啊。。
大概就是求Bessie站在哪里逃不出去。。
当然可以暴力。。100000，显然只能用O(nlogn)，官方的标程也是用这个复杂度的。
假如Bessie从一个很低的区间一直往外冲，结果撞在了很高的一个区间的干草堆上。如果从低区间往高区间求解，不是很亏吗。。于是从高区间向低区间求解↓
先把干草堆的高度排递减序，将每个高度值插入**set**，在set里面二分找它左右相邻的干草堆，
如果这个区间正好能把Bessie拦住，就把这个区间内的干草堆标记一下，以后就不用再标记了。
这里采用左闭右开，将左边的干草堆标记，右边的不标记。
还有一点，数据达到1e9，要离散化。（lz用map）
时间复杂度就是O(nlogn)，但由于用了map常数会大跑得慢。。

# 代码：
```
#include<bits/stdc++.h>
using namespace std;
typedef long long LL;
inline int read()
{
    int x=0;char ch=getchar();
    while(ch<'0'||ch>'9'){ch=getchar();}
    while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
    return x;
}
int n,pos[100008],l,r,ans;
struct node{
	int s,p;
}a[100008];
map<int,int> m;
set<int> s;
set<int>::iterator si;
bool vis[100008];
inline bool cmp(const int a,const int b){return a<b;}
inline bool cmp2(const node a,const node b){return a.s>b.s;}
int main()
{
    n=read();
	for(int i=1;i<=n;i++){
		a[i].s=read();a[i].p=read();
		pos[i]=a[i].p;
	}
	sort(pos+1,pos+n+1,cmp);
	for(int i=1;i<=n;i++)	m[pos[i]]=i;
	sort(a+1,a+n+1,cmp2);
	s.insert(a[1].p);
	for(int i=2;i<=n;i++){
		if(*s.begin()<a[i].p){
			si=--s.upper_bound(a[i].p);
			l=m[*si];r=m[a[i].p];
			if(pos[r]-pos[l]<=a[i].s&&!vis[l]){
				for(int j=l;j<r;j++)	vis[j]=1;
			}
		}
		if(*--s.end()>a[i].p){
			si=s.upper_bound(a[i].p);
			l=m[a[i].p];r=m[*si];
			if(pos[r]-pos[l]<=a[i].s&&!vis[l]){
				for(int j=l;j<r;j++)	vis[j]=1;
			}
		}
		s.insert(a[i].p);
	}
	for(int i=1;i<n;i++){
		if(vis[i])	ans+=pos[i+1]-pos[i];
	}
	cout<<ans;
    return 0;
}
```

欢迎转载或引用，能附上lz的网址就更好啦