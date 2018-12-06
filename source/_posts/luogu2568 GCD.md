---
title: luogu2568 GCD
tag: 数论
---
简单题
考虑枚举gcd，剩下的两个东西互质
想到线性筛欧拉函数
没有注意内存
开了1e7的LL+2·1e7的int+1e7的bool居然没有mle
注意最小的质数是2
欧拉函数只需要到1e7/2即可
这样就不会mle
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
const int N=1e7+8;
int n;
bool not_pri[N];
int pri[N],phi[N/2],num;
LL sum[N/2],ans;
int main()
{
	n=read();
	//phi[1]=sum[1]=1;
	for(int i=2;i<=n;++i){
		if(!not_pri[i]){
			pri[++num]=i;
			if(i<=n/2)	phi[i]=i-1;
		}
		if(i<=n/2)	sum[i]=sum[i-1]+phi[i];
		int j;
		for(j=1;j<=num&&pri[j]<=n/i;++j){
			not_pri[pri[j]*i]=1;
			if(i%pri[j]){
				if(pri[j]<=n/2/i)	phi[pri[j]*i]=phi[i]*(pri[j]-1);
			}
			else{
				if(pri[j]<=n/2/i)	phi[pri[j]*i]=phi[i]*pri[j];
				break;
			}
		}
	}
	for(int i=1;i<=num;++i){
		ans+=sum[n/pri[i]];
	}
	printf("%lld",ans*2+num);
	return 0;
}
```