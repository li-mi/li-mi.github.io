---
title: luogu2396 yyy loves Maths VII
tag: 状压
---
```
很明显的状压
看上去挺卡时间的
实际也很卡时间
开了O2才过
懒得优化了
O(2^n*n)过24
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
const int N=25,mod=1000000007;
int n,m;
int b[3];
int dis[1<<24],f[1<<24];
int ub;
int main()
{
	n=read();
	for(int i=0;i<n;++i){
		dis[1<<i]=read();
	}
	m=read();
	for(int i=1;i<=m;++i){
		b[i]=read();
	}
	ub=1<<n;
	f[0]=1;
	for(int i=1;i<ub;++i){
		int j=i&-i,k=i;
		dis[i]=dis[i^j]+dis[j];
		if(dis[i]==b[1]||dis[i]==b[2])	continue;
		while(k){
			f[i]+=f[i^j];
			if(f[i]>=mod)	f[i]-=mod;
			k^=j;
			j=k&-k;
		}
	}
	printf("%d",f[ub-1]);
	return 0;
}
```