---
title: hihocoder1063 缩地
tag: 
 - 树上dp
 - 背包
---
```
和apple tree很像
但是步数特别大，不能作为一维
发现收益特别小，将收益作为一维
错了一次在于数组开小了
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
const int N=108;
int n,a,b,w;
int v[N];
int ub;
int q,d;

int nume,head[N];
struct node{
	int to,nxt,f;
}e[N<<1];
inline void addedge(int x,int y,int z)
{
	e[++nume]=(node){y,head[x],z};head[x]=nume;
}

int f[N][N<<1][2];//数组开小了！！ 
//f[i][j][0]表示在i的子树里产生j的收益，且回到i点的最少步数
//f[i][j][1]表示在i的子树里产生j的收益的最少步数 
inline void dfs(int x,int fa)
{
	for(int i=head[x];i;i=e[i].nxt){
		if(e[i].to!=fa){
			dfs(e[i].to,x);
			for(int j=ub;j>=1;--j){
				for(int k=j;k;--k){
					f[x][j][0]=min(f[x][j][0],f[e[i].to][k][0]+f[x][j-k][0]+2*e[i].f);
					f[x][j][1]=min(f[x][j][1],f[e[i].to][k][0]+f[x][j-k][1]+2*e[i].f);
					f[x][j][1]=min(f[x][j][1],f[e[i].to][k][1]+f[x][j-k][0]+e[i].f);
					//cout<<x<<' '<<e[i].to<<' '<<j<<' '<<k<<' '<<f[x][j][0]<<' '<<f[x][j][1]<<' '<<f[e[i].to][k][0]<<' '<<f[i][j-k][0]<<'\n';
				}
			}
		}
	}
}
int main()
{
	memset(f,0x3f,sizeof(f));
	n=read();
	for(int i=1;i<=n;++i){
		v[i]=read();
		f[i][v[i]][0]=f[i][v[i]][1]=0;
		ub+=v[i];
	}
	for(int i=1;i<n;++i){
		a=read();b=read();w=read();
		addedge(a,b,w);
		addedge(b,a,w);
	}
	dfs(1,0);
	q=read();
	while(q--){
		d=read();
		for(int j=ub;j>=0;--j){
			if(f[1][j][1]<=d){
				printf("%d\n",j);
				break;
			}
		}
	}
	return 0;
}
```