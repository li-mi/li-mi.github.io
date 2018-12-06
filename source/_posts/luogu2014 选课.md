---
title: luogu2014 选课
tag: 
 - 树上dp
 - 背包
---
```
做完apple tree以后
这题就很简单啦
f[i][j]表示i的子树中选j个课程的最大学分
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
const int N=308;
int n,m;
int fa;

int nume,head[N];
struct node{
	int to,nxt;
}e[N];
inline void addedge(int x,int y)
{
	e[++nume]=(node){y,head[x]};head[x]=nume;
}

int f[N][N];
inline void dfs(int x)
{
	for(int i=head[x];i;i=e[i].nxt){
		dfs(e[i].to);
		for(int j=m;j;--j){
			for(int k=1;k<j;++k){
				f[x][j]=max(f[x][j],f[e[i].to][k]+f[x][j-k]);
			}
		}
	}
}
int main()
{
	n=read();m=read();++m;
	for(int i=1;i<=n;++i){
		fa=read();f[i][1]=read();
		addedge(fa,i);
	}
	dfs(0);
	printf("%d",f[0][m]);
	return 0;
}
```