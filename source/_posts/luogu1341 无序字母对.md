---
title: luogu1341 无序字母对
tag: 欧拉路
---
```
欧拉回路
注意图可能不连通（尽管数据没卡）
要求字典序最小的欧拉回路
所以要用邻接矩阵
注意要倒序输出
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
const int N=58;
int n;
int a[N][N];
char c,ans[N*N];
int u,v,rt;
int d[N],num;
bool vis[N][N],flag;
int cnt;

inline void getc(int &x)
{
	c=getchar();
	while(!(c>='A'&&c<='Z'||c>='a'&&c<='z'))	c=getchar();
	if(c<='Z'){
		x=c-'A'+1;
	}
	else{
		x=c-'a'+27;
	}
}
inline char backc(int x)
{
	if(x<=26){
		return x+'A'-1;
	}
	else{
		return x+'a'-27;
	}
}
inline void dfs(int x)
{
	for(int i=1;i<N;++i){
		if(!vis[x][i]&&a[x][i]){
			vis[x][i]=vis[i][x]=1;
			dfs(i);
		}
	}
	ans[++cnt]=backc(x);
}
int main()
{
	n=read();
	for(int i=1;i<=n;++i){
		getc(u);getc(v);
		a[u][v]=a[v][u]=1;
		++d[u];
		++d[v];
	}
	for(int i=1;i<N;++i){
		if(d[i]&1){
			++num;
		}
	}
	if(num!=0&&num!=2){
		puts("No Solution");
		return 0;
	}
	for(int i=1;i<N;++i){
		if(d[i]){
			if(num==0){
				rt=i;
				break;
			}
			else{
				if(d[i]%2){
					rt=i;
					break;
				}
			}
		}
	}
	dfs(rt);
	if(cnt!=n+1){
		puts("No Solution");
	}
	else{
		for(int i=cnt;i;--i){
			printf("%c",ans[i]);
		}
	}
	return 0;
}
```