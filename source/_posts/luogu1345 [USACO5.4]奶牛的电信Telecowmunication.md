---
title: luogu1345 [USACO5.4]奶牛的电信Telecowmunication
tag: 网络流
---
```
看上去像网络流
写了一发
80分
还以为小细节挂了
其实整个程序都是错的。。

是求最小的割点数
转化成求最小的割边数（最小割）
每个点拆成两个点
i - 1 - i'
原图中的边连INF
这样只会割掉1的边
相当于割掉了这个点
```
<!--more-->
```
#include<bits/stdc++.h>
#define mp make_pair
#define pb push_back
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
const int N=108,M=608;
int n,m,s,t;
int x,y;
int nume=1,head[N*2],cur[N*2];
struct node{
	int to,nxt,f;
}e[M*4+N*2];
inline void addedge(int x,int y,int z)
{
	e[++nume]=(node){y,head[x],z};head[x]=nume;
	e[++nume]=(node){x,head[y],0};head[y]=nume;
}
int dis[N*2];
int q[N*2],he,ta;
inline bool bfs()
{
	memset(dis,0,sizeof(dis));
	dis[s]=1;
	he=1;ta=2;
	q[1]=s;
	while(he!=ta){
		int x=q[he];
		++he;
		for(int i=head[x];i;i=e[i].nxt){
			if(e[i].f&&!dis[e[i].to]){
				dis[e[i].to]=dis[x]+1;
				if(e[i].to==t)	return 1;
				q[ta]=e[i].to;
				++ta;
			}
		}
	}
	return 0;
}
inline int dfs(int x,int low)
{
	if(x==t||!low)	return low;
	int flow=0,tmp;
	for(int &i=cur[x];i;i=e[i].nxt){
		if(dis[e[i].to]==dis[x]+1&&(tmp=dfs(e[i].to,min(low,e[i].f)))){
			flow+=tmp;
			low-=tmp;
			e[i].f-=tmp;
			e[i^1].f+=tmp;
			if(!low)	return flow;
		}
	}
	if(low)	dis[x]=0;
	return flow;
}
inline int dinic()
{
	int maxflow=0;
	while(bfs()){
		for(int i=1;i<=n*2;++i)	cur[i]=head[i];
		maxflow+=dfs(s,INT_MAX);
	}
	return maxflow;
}
int main()
{
	n=read();m=read();s=read();t=read();
	s+=n;
	for(int i=1;i<=n;++i){
		if(i!=s-n&&i!=t){
			addedge(i,n+i,1);
		}
	}
	for(int i=1;i<=m;++i){
		x=read();y=read();
		addedge(x+n,y,INT_MAX);
		addedge(y+n,x,INT_MAX);
	}
	printf("%d",dinic());
	return 0;
}
```