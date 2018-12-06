---
title: luoguP2149 [SDOI2009]Elaxia的路线
tag: 最短路
---
```
显然他们只会在一段连续的点上相遇
先求出每个点是否在最短路上
然后暴力枚举起点和终点
这样就a了
但过不了bzoj上提供的额外两组数据
因为两个点在最短路上不代表它们在同一条最短路上
还要判断它们的距离差是否相等
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
const int N=1508;
int n,m;
int x[3],y[3];
int u,v,l;
int nume,head[N];
struct node{
	int to,nxt,f;
}e[N*N*2];
inline void addedge(int x,int y,int z)
{
	e[++nume]=(node){y,head[x],z};head[x]=nume;
}
int dis[3][N];
int q[N],he,ta;
bool inq[N],ina[3][N];
int ans;
inline void spfa(int t)
{
	memset(inq,0,sizeof(inq));
	dis[t][x[t]]=0;
	he=1;ta=2;
	q[1]=x[t];
	inq[x[t]]=1;
	while(he!=ta){
		int x=q[he];
		inq[x]=0;
		++he;
		if(he==1503)	he=1;
		for(int i=head[x];i;i=e[i].nxt){
			if(dis[t][e[i].to]>dis[t][x]+e[i].f){
				dis[t][e[i].to]=dis[t][x]+e[i].f;
				if(!inq[e[i].to]){
					q[ta]=e[i].to;
					inq[e[i].to]=1;
					++ta;
					if(ta==1503)	ta=1;
				}
			}
		}
	}
	he=1;ta=2;
	q[1]=y[t];
	inq[y[t]]=1;
	while(he!=ta){
		int x=q[he];
		++he;
		ina[t][x]=1;
		for(int i=head[x];i;i=e[i].nxt){
			if(dis[t][x]-e[i].f==dis[t][e[i].to]){
				if(!inq[e[i].to]){
					q[ta]=e[i].to;
					inq[e[i].to]=1;
					++ta;
				}
			}
		}
	}
}
int main()
{
	memset(dis,0x3f,sizeof(dis));
	n=read();m=read();x[1]=read();y[1]=read();x[2]=read();y[2]=read();
	for(int i=1;i<=m;++i){
		u=read();v=read();l=read();
		addedge(u,v,l);
		addedge(v,u,l);
	}
	spfa(1);
	spfa(2);
	for(int i=1;i<=n;++i){
		if(ina[1][i]&&ina[2][i]){
			for(int j=1;j<=n;++j){
				if(i!=j&&ina[1][j]&&ina[2][j]&&abs(dis[1][i]-dis[1][j])==abs(dis[2][i]-dis[2][j])){
					ans=max(ans,abs(dis[1][i]-dis[1][j]));
				}
			}
		}
	}
	printf("%d",ans);
	return 0;
}
```