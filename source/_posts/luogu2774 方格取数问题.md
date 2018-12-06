---
title: luogu2774 方格取数问题
tag:
 - 网络流
---
```
对最小割还是不熟
答案为所有的值减去掉的最小值。
去掉的原因是相邻格子不能同时取。
黑白染色，也就是要么不取黑，要么不取白
相邻的连边 s - black's key - black - INF - white - white's key - t
那最小割就是去掉的最小值
64分的错误原因是用u的奇偶性判断黑白了
```
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
const int N=108;
int n,m,u,v,s,t;
int a[N][N];
int dx[4]={-1,0,0,1},dy[4]={0,-1,1,0},tx,ty;
LL ans;
inline int getid(int x,int y)
{
	return (x-1)*m+y;
}

int nume=1,head[N*N],cur[N*N];
struct node{
	int to,nxt,f;
}e[N*N*4];
inline void addedge(int x,int y,int z)
{
	e[++nume]=(node){y,head[x],z};head[x]=nume;
	e[++nume]=(node){x,head[y],0};head[y]=nume;
}

int dis[N*N];
int q[N*N],he,ta;
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
inline LL dinic()
{
	LL maxflow=0;
	while(bfs()){
		for(int i=0;i<=t;++i){
			cur[i]=head[i];
		}
		maxflow+=dfs(s,INT_MAX);
	}
	return maxflow;
}
int main()
{
	n=read();m=read();
	s=0;t=n*m+1;
	for(int i=1;i<=n;++i){
		for(int j=1;j<=m;++j){
			a[i][j]=read();
			ans+=a[i][j];
		}
	}
	for(int i=1;i<=n;++i){
		for(int j=1;j<=m;++j){
			u=getid(i,j);
			if((i+j)%2==0){
				addedge(s,u,a[i][j]);
				for(int k=0;k<4;++k){
					tx=i+dx[k];
					ty=j+dy[k];
					if(tx<1||tx>n||ty<1||ty>m)	continue;
					v=getid(tx,ty);
					addedge(u,v,INT_MAX);
				}
			}
			else{
				addedge(u,t,a[i][j]);
			}
		}
	}
	printf("%lld",ans-dinic());
	return 0;
}
```