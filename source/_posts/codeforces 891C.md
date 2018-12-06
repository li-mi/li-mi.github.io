---
title: codeforces 891C
date: 
tags:
mathjax: true
---


# 题意：
891C. Envy
time limit per test: 2 seconds
memory limit per test: 256 megabytes
input: standard input
output: standard output

For a connected undirected weighted graph $G$, MST (minimum spanning tree) is a subgraph of $G$ that contains all of $G$'s vertices, is a tree, and sum of its edges is minimum possible.

You are given a graph $G$. If you run a MST algorithm on graph it would give you only one MST and it causes other edges to become jealous. You are given some queries, each query contains a set of edges of graph $G$, and you should determine whether there is a MST containing all these edges or not.
<!--more-->
### Input
The first line contains two integers $n$, $m$ (2≤$n$,$m$≤5·10$^5$, $n$-1≤$m$) — the number of vertices and edges in the graph and the number of queries.

The $i$-th of the next $m$ lines contains three integers $ui$, $vi$, $wi$ ($ui$≠$vi$, 1≤$wi$≤5·10$^5$) — the endpoints and weight of the $i$-th edge. There can be more than one edges between two vertices. It's guaranteed that the given graph is connected.

The next line contains a single integer $q$ (1≤$q$≤5·10$^5$) — the number of queries.

$q$ lines follow, the $i$-th of them contains the $i$-th query. It starts with an integer $ki$ (1≤$ki$≤$n$-1) — the size of edges subset and continues with $ki$ distinct space-separated integers from 1 to $m$ — the indices of the edges. It is guaranteed that the sum of $ki$ for 1≤$i$≤$q$ does not exceed 5·10$^5$.

### Output
For each query you should print "YES" (without quotes) if there's a MST containing these edges and "NO" (of course without quotes again) otherwise.

### Example
|input|
|:-|
|5 7|
|1 2 2|
|1 3 2|
|2 3 1|
|2 4 1|
|3 4 1|
|3 5 2|
|4 5 2|
|4|
|2 3 4|
|3 3 4 5|
|2 1 7|
|2 1 2|

|output|
|:-|
|YES|
|NO|
|YES|
|NO|



# 题解：
看来lz太弱了，此题被初二大佬yht水过。
主要是要想到离线处理。把所有询问的边从小到大考虑，每次考虑一堆相同权值的边。
假如当前边的权值为x，则小于x的所有边都已经进行过了kruskal，加进了并查集。
对含有权值为x的询问，我们将其中所有权值为x的加入并查集，若出现环，则"NO"。
然后要将本次操作涉及的合并还原（重要！）。
在完成所有有权值为x的询问后，将这些边真正的加入并查集。
复杂度$O(mlogm+\sum ki)$
//函数递归的函数名不能写错啊~~



# 代码：
```
#include<bits/stdc++.h>
using namespace std;
typedef long long LL;
typedef pair<int,int> PII;
inline LL read()
{
	LL x=0,f=1;char ch=getchar();
	while(ch<'0'||ch>'9'){if(ch=='-')f=-1;ch=getchar();}
	while(ch>='0'&&ch<='9'){x=(x<<1)+(x<<3)+(ch^48);ch=getchar();}
	return x*f;
}
int n,m;
int u[500008],v[500008],w[500008],id[500008];
int q,k,x;
int p1,p2,tmp;
int fa[500008];
map<int,vector<int> > m1,m2[500008];
int qid,l,r;
bool vis[500008];
int top,val[500008];
int *s[500008];

inline int find(int x){
	return x==fa[x]?x:fa[x]=find(fa[x]);
}
inline int find2(int x){
	if(x==fa[x])	return x;
	s[++top]=fa+x;
	val[top]=fa[x];
	fa[x]=find2(fa[x]);//调用find2，写成find错了好几次
	return fa[x];
}
inline void del()
{
	while(top){
		(*s[top])=val[top];--top;
	}
}
inline bool cmp(const int a,const int b){
	return w[a]<w[b];
}
int main()
{
	n=read();m=read();
	for(int i=1;i<=n;++i){
		fa[i]=i;
	}
	for(int i=1;i<=m;++i){
		u[i]=read();v[i]=read();w[i]=read();
		id[i]=i;
	}
	sort(id+1,id+m+1,cmp);
	q=read();
	for(int i=1;i<=q;++i){
		k=read();
		for(int j=1;j<=k;++j){
			x=read();
			if(m1[w[x]].size()==0||m1[w[x]][m1[w[x]].size()-1]!=i)	m1[w[x]].push_back(i);
			m2[i][w[x]].push_back(x);
		}
	}
	for(int i=1;i<=m;++i){
		l=i;r=i;
		while(w[id[r+1]]==w[id[l]]&&r+1<=m)	++r;
		
		for(int j=0;j<m1[w[id[i]]].size();++j){
			qid=m1[w[id[i]]][j];
			if(vis[qid])	continue;
			for(int k=0;k<m2[qid][w[id[i]]].size();++k){
				tmp=m2[qid][w[id[i]]][k];
				p1=find2(u[tmp]);
				p2=find2(v[tmp]);
				if(p1!=p2){
					s[++top]=fa+p1;
					val[top]=fa[p1];
					fa[p1]=p2;
				}
				else{
					vis[qid]=1;
					break;
				}
			}
			del();
		}
		
		for(int j=l;j<=r;++j){
			tmp=id[j];
			p1=find(u[tmp]);
			p2=find(v[tmp]);
			if(p1!=p2){
				fa[p1]=p2;
			}
		}
		i=r;
	}
	for(int i=1;i<=q;++i){
		puts(vis[i]?"NO":"YES");
	}
	return 0;
} 
```
欢迎转载或引用，能附上lz的网址就更好啦