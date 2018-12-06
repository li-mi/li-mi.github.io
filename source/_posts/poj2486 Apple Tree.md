---
title: poj2486 Apple Tree
tag: 
 - 树上dp 
 - 背包
---
```
好久没写树上dp和背包了
看了好久题解
每个点权都是非负的
所以多余的步数走了也不会使答案变差
每一步可以选择停在原地不动
f[i][j][0]表示在节点i的子树中走j步，且最终回到i的最大收益
f[i][j][1]表示在节点i的子树中走j步的最大收益
因为一个子树重复进去是没有收益的
所以相当于一个01背包
j要从大到小枚举
```
<!--more-->
```
#include<iostream>
#include<cstdio>
#include<cstring>
#include<climits>
#include<queue>
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
const int N=108,K=208;
int n,k,u,v;
int w[N];

int nume,head[N];
struct node{
	int to,nxt;
}e[N<<1];
inline void addedge(int x,int y)
{
	e[++nume]=(node){y,head[x]};head[x]=nume;
}
int f[N][K][2];
//f[i][j][0]表示在节点i的子树中走j步，且最终回到i的最大收益
//f[i][j][1]表示在节点i的子树中走j步的最大收益 
//多余的步数并不影响答案，每一步可以选择停在原地不动 
inline void dfs(int x,int fa)
{
	for(int j=0;j<=k;++j)	f[x][j][0]=f[x][j][1]=w[x];
	for(int i=head[x];i;i=e[i].nxt){
		if(e[i].to!=fa){
			dfs(e[i].to,x);
			for(int j=k;j>=1;--j){
				for(int kk=j-1;kk>=0;--kk){//从小到大和从大到小是没有影响的
					if(j-kk>=2){
						f[x][j][0]=max(f[x][j][0],f[e[i].to][kk][0]+f[x][j-kk-2][0]);
						f[x][j][1]=max(f[x][j][1],f[e[i].to][kk][0]+f[x][j-kk-2][1]);
					}
					f[x][j][1]=max(f[x][j][1],f[e[i].to][kk][1]+f[x][j-kk-1][0]);
				}
			}
		}
	}
}
int main()
{
	while(scanf("%d%d",&n,&k)!=EOF){
		nume=1;
		memset(head,0,sizeof(head));
		
		for(int i=1;i<=n;++i){
			scanf("%d",&w[i]);
		}
		for(int i=1;i<n;++i){
			scanf("%d%d",&u,&v);
			addedge(u,v);
			addedge(v,u);
		}
		dfs(1,0);
		printf("%d\n",f[1][k][1]);//一个优秀的答案总是不会回家的 
	}
	return 0;
}
```
```
还有一种写法
不合法的情况直接为0
赋初值的话只有f[i][0][0]和f[i][0][1]
最后在所有步数里面取max就可以了
```
```
#include<iostream>
#include<cstdio>
#include<cstring>
#include<climits>
#include<queue>
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
const int N=108,K=208;
int n,k,u,v;
int w[N];
int ans;
int nume,head[N];
struct node{
	int to,nxt;
}e[N<<1];
inline void addedge(int x,int y)
{
	e[++nume]=(node){y,head[x]};head[x]=nume;
}
int f[N][K][2];
inline void dfs(int x,int fa)
{
	//for(int j=0;j<=k;++j)	f[x][j][0]=f[x][j][1]=w[x];
	f[x][0][0]=f[x][0][1]=w[x];
	for(int i=head[x];i;i=e[i].nxt){
		if(e[i].to!=fa){
			dfs(e[i].to,x);
			for(int j=k;j>=1;--j){
				for(int kk=j-1;kk>=0;--kk){
					if(j-kk>=2){
						f[x][j][0]=max(f[x][j][0],f[e[i].to][kk][0]+f[x][j-kk-2][0]);
						f[x][j][1]=max(f[x][j][1],f[e[i].to][kk][0]+f[x][j-kk-2][1]);
					}
					f[x][j][1]=max(f[x][j][1],f[e[i].to][kk][1]+f[x][j-kk-1][0]);
				}
			}
		}
	}
}
int main()
{
	while(scanf("%d%d",&n,&k)!=EOF){
		nume=1;
		memset(head,0,sizeof(head));
		memset(f,0,sizeof(f));
		
		for(int i=1;i<=n;++i){
			scanf("%d",&w[i]);
		}
		for(int i=1;i<n;++i){
			scanf("%d%d",&u,&v);
			addedge(u,v);
			addedge(v,u);
		}
		dfs(1,0);
		//printf("%d\n",f[1][k][1]);
		ans=0;
		for(int j=0;j<=k;++j){
			ans=max(ans,f[1][j][1]);
		}
		printf("%d\n",ans);
	}
	return 0;
}
```