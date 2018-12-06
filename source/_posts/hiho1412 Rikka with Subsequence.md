---
title: hiho1412 Rikka with Subsequence
tag: 
- dp
---
```
序列自动机本质上就是dp
所以这题考虑dp
f[i]表示到以i结尾的子序列的合法的概率数
c[i][ch]表示到第i位为止没有用到的ch全被删除的子序列的合法的概率数
发现c的第一维无意义可以省去
dp的含义要搞清楚
```
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
const int N=500008,mod=998244353;
int ni=828542813; 
int n;
char s[N];
int a[N];
int f[N],c[28],ans;
inline LL ksm(LL x,int y)
{
	LL tmp=1;
	while(y){
		if(y&1)	tmp=tmp*x%mod;
		x=x*x%mod;
		y>>=1;
	}
	return tmp;
}
int main()
{
	n=read();
	scanf("%s",s+1);
	for(int i=1;i<=n;++i){
		a[i]=read();
	}
	for(int i=0;i<26;++i){
		c[i]=1;
	}
	f[0]=1;
	for(int i=1;i<=n;++i){
		f[i]=1ll*c[s[i]-'a']*(100-a[i])%mod*ni%mod;
		for(int j=0;j<26;++j){
			if(s[i]-'a'==j){
				c[j]=(1ll*c[j]*a[i]%mod*ni%mod+f[i])%mod;
			}
			else{
				c[j]=(c[j]+f[i])%mod;
			}
		}
	}
	for(int i=0;i<=n;++i){
		ans=(ans+f[i])%mod;
	}
	printf("%d",ans);
	return 0;
}
```