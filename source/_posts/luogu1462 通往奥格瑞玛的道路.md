---
title: luogu1462 通往奥格瑞玛的道路
tag:
 - 二分
 - 最短路
---
```
很久没写最短路了
题目有点绕
看懂之后就是一个二分
dijk在开O2下比spfa快（正常也应该快啊。。）
deque更快的（O2下，可queue居然是最快的。。）
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
const int N=10008,M=50008;
int n,m,b;
int a[N];
int x,y,z;
int nume,head[N];
struct node{
	int to,nxt,f;
}e[M<<1];
inline void addedge(int x,int y,int z)
{
	e[++nume]=(node){y,head[x],z};head[x]=nume;
}
int l,r,mid,ans=INT_MAX;
int dis[N];
int q[N],he,ta;
bool inq[N];
inline bool ok()
{
	memset(dis,0x3f,sizeof(dis));
	dis[1]=0;
	he=1;ta=2;
	q[1]=1;
	inq[1]=1;
	while(he!=ta){
		int x=q[he];
		inq[x]=0;
		++he;
		if(he==10003)	he=1;
		for(int i=head[x];i;i=e[i].nxt){
			if(a[e[i].to]<=mid&&dis[e[i].to]>dis[x]+e[i].f){
				dis[e[i].to]=dis[x]+e[i].f;
				if(!inq[e[i].to]){
					q[ta]=e[i].to;
					inq[e[i].to]=1;
					++ta;
					if(ta==10003)	ta=1;
				}
			}
		}
	}
	if(dis[n]<=b)	return 1;
	else	return 0;
}
int main()
{
	n=read();m=read();b=read();
	for(int i=1;i<=n;++i){
		a[i]=read();
		r=max(r,a[i]);
	}
	l=a[1];
	for(int i=1;i<=m;++i){
		x=read();y=read();z=read();
		addedge(x,y,z);
		addedge(y,x,z);
	}
	while(l<=r){
		mid=(r-l)/2+l;
		if(ok()){
			ans=mid;
			r=mid-1;
		}
		else{
			l=mid+1;
		}
	}
	if(ans!=INT_MAX)	printf("%d",ans);
	else	puts("AFK");
	return 0;
}
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
const int N=10008,M=50008;
int n,m,b;
int a[N];
int x,y,z;
int nume,head[N];
struct node{
	int to,nxt,f;
}e[M<<1];
inline void addedge(int x,int y,int z)
{
	e[++nume]=(node){y,head[x],z};head[x]=nume;
}
int l,r,mid,ans=INT_MAX;
int dis[N];
priority_queue<PII,vector<PII>,greater<PII> > q;
bool vis[N];
inline bool ok()
{
	memset(dis,0x3f,sizeof(dis));
	memset(vis,0,sizeof(vis));
	dis[1]=0;
	q.push(mp(0,1));
	while(!q.empty()){
		int x=q.top().second;
		q.pop();
		if(vis[x])	continue;
		vis[x]=1;
		for(int i=head[x];i;i=e[i].nxt){
			if(a[e[i].to]<=mid&&dis[e[i].to]>dis[x]+e[i].f){
				dis[e[i].to]=dis[x]+e[i].f;
				q.push(mp(dis[e[i].to],e[i].to));
			}
		}
	}
	if(dis[n]<=b)	return 1;
	else	return 0;
}
int main()
{
	n=read();m=read();b=read();
	for(int i=1;i<=n;++i){
		a[i]=read();
		r=max(r,a[i]);
	}
	l=a[1];
	for(int i=1;i<=m;++i){
		x=read();y=read();z=read();
		addedge(x,y,z);
		addedge(y,x,z);
	}
	while(l<=r){
		mid=(r-l)/2+l;
		if(ok()){
			ans=mid;
			r=mid-1;
		}
		else{
			l=mid+1;
		}
	}
	if(ans!=INT_MAX)	printf("%d",ans);
	else	puts("AFK");
	return 0;
}
```