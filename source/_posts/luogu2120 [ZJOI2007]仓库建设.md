---
title: luogu2120 [ZJOI2007]仓库建设
tag: 斜率优化
---
做特别行动队的时候发现这题的题解还没写
这才是第一道斜率优化啊
当时懵懵懂懂对着题解写的
现在明白了

想想全tm是套路
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
const int N=1000008;
int n;
int x[N],c[N];
LL s[N],t[N],f[N];
int q[N],he,ta;
inline double rate(int x,int y)
{
	return (f[y]+t[y]-f[x]-t[x])*1.0/(s[y]-s[x]);
}
int main()
{
	n=read();
	for(int i=1;i<=n;++i){
		x[i]=read();s[i]=read();c[i]=read();
		t[i]=t[i-1]+s[i]*x[i];
		s[i]+=s[i-1];
	}
	he=1;ta=1;q[1]=0;
	for(int i=1;i<=n;++i){
		while(he+1<=ta&&x[i]>rate(q[he],q[he+1]))	++he;
		f[i]=f[q[he]]+c[i]+(s[i]-s[q[he]])*x[i]-t[i]+t[q[he]];
		while(he+1<=ta&&rate(q[ta],i)<rate(q[ta-1],q[ta]))	--ta;
		q[++ta]=i;
	}
	printf("%lld",f[n]);
	return 0;
}
```