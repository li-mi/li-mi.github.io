---
title: luogu2764最小路径覆盖问题
tag: 
 - 网络流
---
```
n个点就是n条路径
s-1-x- [有边]*1 -y - 1 -t
每次将两个点连在一起就减少1条边，增加1点流量
所以最小路径数=n-最大流
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
const int N=308,M=6008; 
int n,m,u,v,s,t,ans;

int nume,head[N];
struct node{
	int to,nxt,f;
}e[M*2+N*4];
inline void addedge(int x,int y,int z)
{
	e[++nume]=(node){y,head[x],z};head[x]=nume;
	e[++nume]=(node){x,head[y],0};head[y]=nume;
}

int cur[N],dis[N];
int he,ta,q[N];
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
			e[i^1].f+=tmp;//´ò´í+- 
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
		for(int i=0;i<=t;++i){
			cur[i]=head[i];
		}
		maxflow+=dfs(s,INT_MAX); 
	}
	return maxflow;
}
int nxt[N],ind[N];
inline void print()
{
	/*for(u=0;u<=t;++u){
		cout<<"!!"<<u<<'\n';
		for(int i=head[u];i;i=e[i].nxt){
			cout<<e[i].to<<' '<<e[i].f<<'\n'; 
		}
	}*/
	for(u=1;u<=n;++u){
		for(int i=head[u];i;i=e[i].nxt){
			if(i%2==0&&e[i].to>n&&e[i].f==0){
				nxt[u]=e[i].to-n;
				++ind[e[i].to-n];
			}
		}
	}
	for(u=1;u<=n;++u){
		if(!ind[u]){
			v=u;
			while(v){
				printf("%d ",v);
				v=nxt[v];
			}
			puts("");
		}
	}
}
int main()
{
	nume=1;
	n=read();m=read();
	s=0;t=n+n+1; 
	for(int i=1;i<=m;++i){
		u=read();v=read();
		addedge(u,n+v,1); 
	}
	for(int i=1;i<=n;++i){
		addedge(s,i,1);
		addedge(n+i,t,1);
	}
	
	ans=dinic();
	print();
	printf("%d",n-ans);
	return 0;
}
```