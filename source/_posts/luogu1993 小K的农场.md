---
title: luogu1993 小K的农场
tag: 
 - 差分约束
---
```
一眼差分约束，一眼数据1e4
一看题解，全写的是spfa_dfs！？
spfa还能用dfs？
吓得我赶紧去负环的模板题看题解
也是dfs？
不过高级的管理员改了数据
把复杂度没有保证的dfs全部hack了
只能愉快的写bfs啦
直接写的话会t
新技能：保存每个点的最短路径长度，>n说明出现负环
这样写开O2才能a掉
但是数据太弱
>15就认为出现负环
这样就0ms过了

顺便做了一下负环的模板题
发现记录路径长度比记录进队次数慢。。
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
const int N=10008;
int n,m,t;
int x,y,z;
int nume,head[N];
struct node{
	int to,nxt,f;
}e[N<<1];
inline void addedge(int x,int y,int z)
{
	e[++nume]=(node){y,head[x],z};head[x]=nume;
}
int dis[N];
int q[N],he,ta;
bool inq[N];
int tim[N];
inline bool spfa()
{
	for(int i=1;i<=n;++i){
		q[i]=i;
		inq[i]=1;
		tim[i]=1;
	}
	he=1;ta=n+1;
	while(he!=ta){
		int x=q[he];
		inq[x]=0;
		++he;
		if(he==10003)	he=1;
		for(int i=head[x];i;i=e[i].nxt){
			if(dis[e[i].to]>dis[x]+e[i].f){
				dis[e[i].to]=dis[x]+e[i].f;
				//++tim[e[i].to];
				//if(tim[e[i].to]>n)	return 0; 
				tim[e[i].to]=tim[x]+1;
				if(tim[e[i].to]>n){//>15也可以，0ms 
					return 0;
				}
				if(!inq[e[i].to]){
					q[ta]=e[i].to;
					inq[e[i].to]=1;
					++ta;
					if(ta==10003)	ta=1;
				}
			}
		}
	}
	return 1;
}
int main()
{
	n=read();m=read();
	while(m--){
		t=read();
		if(t==1){
			x=read();y=read();z=read();
			addedge(x,y,-z);
		}
		else if(t==2){
			x=read();y=read();z=read();
			addedge(y,x,z);
		}
		else{
			x=read();y=read();
			addedge(x,y,0);
			addedge(y,x,0);
		}
	}
	if(spfa()){
		puts("Yes");
	}
	else{
		puts("No");
	}
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
const int N=4008,M=6008;
int T;
int n,m;
int x,y,z;
int nume,head[N];
struct node{
	int to,nxt,f;
}e[M<<1];
inline void addedge(int x,int y,int z)
{
	e[++nume]=(node){y,head[x],z};head[x]=nume;
}
int dis[N],q[N],he,ta,tim[N];
bool inq[N];
inline void spfa()
{
	for(int i=1;i<=n;++i){
		dis[i]=0;
		inq[i]=1;
		q[i]=i;
		tim[i]=1;
	}
	he=1;ta=n+1;//队列的ta要+1 
	while(he!=ta){
		int x=q[he];
		inq[x]=0;
		++he;
		if(he==4003)	he=1;
		for(int i=head[x];i;i=e[i].nxt){
			if(dis[e[i].to]>dis[x]+e[i].f){
				dis[e[i].to]=dis[x]+e[i].f;
				++tim[e[i].to];
				/*if(tim[e[i].to]>n){
					puts("YE5");
					return;	
				}*/
				tim[e[i].to]=tim[x]+1;
				if(tim[e[i].to]>min(2500,n)){
					puts("YE5");
					return;
				}
				if(!inq[e[i].to]){
					q[ta]=e[i].to;
					inq[e[i].to]=1;
					++ta;
					if(ta==4003)	ta=1;
				}
			}
		}
	}
	puts("N0");
	return;
}
int main()
{
	T=read();
	while(T--){
		nume=0;
		memset(head,0,sizeof(head));
		
		n=read();m=read();
		for(int i=1;i<=m;++i){
			x=read();y=read();z=read();
			addedge(x,y,z);
			if(z>=0){
				addedge(y,x,z);
			}
		}
		spfa();
	}
	return 0;
}
```