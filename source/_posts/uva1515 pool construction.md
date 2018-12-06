---
title: uva1515 pool construction
tag: 网络流
---
```
建图很妙啊
最小割的应用
注意所有相邻的格子都要连边
只要相邻的格子所属的集合不同，这条边就要被割掉
注意不要打错变量名
（在vjudge上的两个oj交a了，还有一个oj上mle，将空间缩小到1/4就a了，很迷）
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
const int N=2508;
int T;
int w,h,d,f,b;
int s,t,x,y;
int ans;
char c,a[58][58];
int nume,head[N],cur[N];
struct node{
	int to,nxt,f;
}e[N*N*4];
int dx[2]={0,1},dy[2]={1,0},tx,ty;
inline int getid(int x,int y)
{
	return (x-1)*w+y;
}
inline void addedge(int x,int y,int z)
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
		for(int i=s;i<=t;++i){
			cur[i]=head[i];
		}
		maxflow+=dfs(s,INT_MAX);
	}
	return maxflow;
}
int main()
{
	T=read();
	while(T--){
		ans=0;
		nume=1;
		memset(head,0,sizeof(head));
		
		w=read();h=read();
		s=0;t=w*h+1;
		d=read();f=read();b=read();
		for(int i=1;i<=h;++i){
			for(int j=1;j<=w;++j){
				c=getchar();
				while(c!='.'&&c!='#')	c=getchar();
				a[i][j]=c;
				if((i==1||i==h||j==1||j==w)){
					if(a[i][j]=='.'){
						a[i][j]='#';
						ans+=f;
					}
					x=getid(i,j);
					addedge(s,x,INT_MAX);
					addedge(x,s,0);
				}
			}
		}
		for(int i=1;i<h;++i){
			for(int j=1;j<w;++j){
				x=getid(i,j);
				if(i!=1&&j!=1){
					if(a[i][j]=='#'){
						addedge(s,x,d);
						addedge(x,s,0);
					}
					else{
						addedge(x,t,f);
						addedge(t,x,0);
					}
				}
				for(int k=0;k<2;++k){
					tx=i+dx[k];
					ty=j+dy[k];
					if(tx<1||tx>h||ty<1||ty>w)	continue;
					y=getid(tx,ty);
					addedge(x,y,b);
					addedge(y,x,b);
				}
			}
		}
		printf("%d\n",ans+dinic());
	}
	return 0;
}
```