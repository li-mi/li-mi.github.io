---
title: luogu2765魔术球问题
tag:
 - 贪心
 - 网络流
---
```
正确的做法是网络流
若i+j为平方数(i<j), i - 1 - j
转化成最小路径覆盖
当最小路径覆盖>n时，前一个为答案
记得在上一次的残余网络里跑，不用清空流量
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
const int N=3508;
int n,s,t;
int ans;
int nume=1,head[N],cur[N];
struct node{
	int to,nxt,f;
}e[N*N];
inline void addedge(int x,int y,int z)
{
	e[++nume]=(node){y,head[x],z};head[x]=nume;
	e[++nume]=(node){x,head[y],z};head[y]=nume;
}

int q[N],he,ta;
int dis[N];
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
		if(dis[e[i].to]==dis[x]+1&&e[i].f&&(tmp=dfs(e[i].to,min(low,e[i].f)))){
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
inline int dinic(int x)
{
	int maxflow=0;
	while(bfs()){
		cur[0]=head[0];
		cur[t]=head[t];
		for(int i=1;i<=x;++i){
			cur[i]=head[i];
			cur[i+1700]=head[i+1700];
		}
		maxflow+=dfs(s,INT_MAX);
	}
	return maxflow;
}

int nxt[N],d[N];
inline void print(int x)
{
	for(int u=1;u<=x;++u){
		for(int i=head[u];i;i=e[i].nxt){
			if(i%2==0&&e[i].f==0){
				nxt[u]=e[i].to-1700;
				++d[e[i].to-1700];
				break;
			}
		}
	}
	for(int u=1;u<=x;++u){
		if(d[u])	continue;
		int i=u;
		while(i){
			printf("%d ",i);
			i=nxt[i];
		}
		puts("");
	}
} 
bool vis[4000];
int main()
{
	for(int i=1;i<=60;++i){
		vis[i*i]=1;
	}
	n=read();
	s=0;t=3500;
	for(int i=1;;++i){
		addedge(s,i,1);
		addedge(1700+i,t,1);
		for(int j=i-1;j;--j){
			if(vis[j+i]){
				addedge(j,1700+i,1);
			}
		}
		ans+=dinic(i);
		if(i-ans>n){
			printf("%d\n",i-1);
			print(i-1); 
			return 0;
		}
	}
	return 0;
}
```
```
贪心
不知道如何证明贪心正确
个人认为当n特别大时能卡掉贪心
```