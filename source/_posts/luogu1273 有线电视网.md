---
title: luogu1273 有线电视网
tag: 
 - 树上dp
 - 背包
---
```
O(n^2)的dp
有一个重要的地方
每个点的循环大小是它子树中用户节点的个数
不加上这个50分
每两个点只会在lca产生贡献
所以n^2
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
const int N=3008;
int n,m;
int x,y,z;

int nume,head[N];
struct node{
	int to,nxt,f;
}e[N<<1];
inline void addedge(int x,int y,int z)
{
	e[++nume]=(node){y,head[x],z};head[x]=nume;
}

int f[N][N];
inline int dfs(int x,int fa)
{
	if(x>n-m){
		return 1;
	}
	int tot=0;
	for(int i=head[x];i;i=e[i].nxt){
		if(e[i].to!=fa){
			tot+=dfs(e[i].to,x);
			for(int j=tot;j;--j){
				for(int k=j;k;--k){
					f[x][j]=max(f[x][j],f[e[i].to][k]+f[x][j-k]-e[i].f);
				}
			}
		}
	}
	return tot;
}
int main()
{
	n=read();m=read();
	for(int i=1;i<=n;++i){
		for(int j=1;j<=n;++j){
			f[i][j]=-0x3f3f3f3f;
		}
	}
	for(int i=1;i<=n-m;++i){
		z=read();
		while(z--){
			x=read();y=read();
			addedge(i,x,y);
			addedge(x,i,y);
		}
	}
	for(int i=n-m+1;i<=n;++i){
		x=read();
		f[i][1]=x;
	}
	dfs(1,0);
	
	for(int j=n;j>=0;--j){
		if(f[1][j]>=0){
			printf("%d",j);
			break;
		}
	}
	return 0;
}
```