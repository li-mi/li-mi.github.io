---
title: poj2987 Firing
tag: 网络流
---
```
最大权闭合子图
最小割一定是简单割，即只割与s或t相连的边
在poj交ce，'LONG_LONG_MAX' was not declared in this scope，不知道为什么
改成INF就过了
用C++编译会ce，G++才能过。。
```
<!--more-->
```
#include<iostream>
#include<cstdio>
#include<cstring>
#include<climits>
#include<queue>
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
const int N=5008,M=60008;
const LL INF=1e16;
int n,m,u,v;
int s,t;
int b[N];
int num;
LL ans;
int nume=1,head[N],cur[N];
struct node{
	int to,nxt;
	LL f;
}e[M*2+N*2];
inline void addedge(int x,int y,LL z)
{
	e[++nume]=(node){y,head[x],z};head[x]=nume;
}
int dis[N];
int q[N],he,ta;
inline bool bfs()
{
	memset(dis,0,sizeof(dis));
	he=1;ta=2;
	q[1]=s;
	dis[s]=1;
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
inline LL dfs(int x,LL low)
{
	if(x==t||!low)	return low;
	LL flow=0,tmp;
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
inline LL dinic()
{
	LL maxflow=0;
	while(bfs()){
		for(int i=s;i<=t;++i){
			cur[i]=head[i];
		}
		maxflow+=dfs(s,INF);
	}
	return maxflow;
}
bool vis[N];
inline void calc(int x)
{
	vis[x]=1;
	++num;
	for(int i=head[x];i;i=e[i].nxt){
		if(e[i].f&&!vis[e[i].to]){
			calc(e[i].to);
		}
	}
}
int main()
{
	n=read();m=read();
	s=0;t=n+1;
	for(int i=1;i<=n;++i){
		b[i]=read();
		if(b[i]<0){
			addedge(i,t,-b[i]);
			addedge(t,i,0);
		}
		else{
			ans+=b[i];
			addedge(s,i,b[i]);
			addedge(i,s,0);
		}
	}
	for(int i=1;i<=m;++i){
		u=read();v=read();
		addedge(u,v,INF);
		addedge(v,u,0);
	}
	ans-=dinic();
	calc(s);
	printf("%d %lld",num-1,ans);
	return 0;
}
```