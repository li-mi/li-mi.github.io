---
title: bzoj2115 [Wc2011] Xor
tag: 线性基
---
线性基最基本的题目了吧
边有权值
图中任意的环都可以走出来（先到环上一点，绕一圈，再回去）
先随便找一条1到n的路径
把环做线性基
错了一次在于某个地方的LL写成int
<!--more-->
```cpp
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
const int N=50008,M=100008;
int n,m;
int x,y;
LL z;
vector<pair<int,LL> > e[N];
bool vis[N];
LL w[N];
LL xhj[64];
LL ans;
inline void inser(LL x)
{
	for(int i=62;i>=0;--i){
		if(!(x>>i))	continue;
		if(!xhj[i]){
			xhj[i]=x;
			return;
		}
		else{
			x^=xhj[i];
		}
	}
}
inline void dfs(int x,int fa)
{
	vis[x]=1;
	for(int i=0;i<e[x].size();++i){
		if(e[x][i].first!=fa){
			if(vis[e[x][i].first]){
				inser(e[x][i].second^w[x]^w[e[x][i].first]);
			}
			else{
				w[e[x][i].first]=w[x]^e[x][i].second;
				dfs(e[x][i].first,x);
			}
		}
	} 
}
int main()
{
	n=read();m=read();
	for(int i=1;i<=m;++i){
		x=read();y=read();z=read();
		e[x].pb(mp(y,z));
		e[y].pb(mp(x,z));
	}
	dfs(1,0);
	ans=w[n];
	for(int i=62;i>=0;--i){
		ans=max(ans,ans^xhj[i]);
	}
	printf("%lld\n",ans);
	return 0;
}
```