---
title: luogu3628 [APIO2010]特别行动队
tag: 斜率优化
---
斜率优化真是套路

$f[i]=min(a[i]*b[j]+c[j]+d[i])$

看到这种带当前i和转移j的二次项的，就是斜率优化了
d[i]最后加可以不管，去掉min，然后整理成y=kx+b的形式

$c[j]=-a[i]*b[j]+f[i]$

k为-a[i]，b为f[i]，(x,y)为(b[j],c[j])
要最大化f[i]，就是维护上凸壳
要最小化f[i]，就是维护下凸壳
注意起点在哪个位置（一般是(0,0)

还可以考虑比较法
考虑1<=k<j<i
假如j的转移更优要满足的条件
然后推一波式子发现和上面的是一样的

然后是重点（敲黑板
如果b[i]随i有单调性（单调递增最好，如果是单调递减可以提负号好理解
* a[i]有单调性，分成两种单调性，一种随着凸壳往后而往后，一种只会停在第一个点（这种情况几乎不会有吧
  复杂度是O(n)
* a[i]没有单调性，那么要二分，复杂度O(nlogn)。以下引用：
  二分做法：假设你要在上凸包上二分找斜率为k的切线。取中间的mid号点，如果mid+1存在且与mid点的斜率小于k，则l=mid+1；如果mid−1存在且与mid点的斜率大于k，则r=mid−1；如果上面两条都不满足，则mid就是切点。[一个很好的斜率优化讲解](http://www.cnblogs.com/MashiroSky/p/6009685.html)
  
如果b[i]没有单调性（糟糕要动态维护凸包）
那么考虑set/手写平衡树/cdq分治，复杂度O(nlogn)
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
int a,b,c;
int sum[N];
int q[N],he,ta;
LL xl; 
LL dp[N];
inline LL calc(int x)
{
	return dp[x]+1ll*a*sum[x]*sum[x]-1ll*b*sum[x];
}
inline double rate(int x,int y)
{
	return (calc(x)-calc(y))/(sum[x]-sum[y]);
}
int main()
{
	n=read();
	a=read();b=read();c=read();
	for(int i=1;i<=n;++i){
		sum[i]=sum[i-1]+read();
	}
	he=ta=1;
	q[1]=0;
	xl=2ll*a;
	for(int i=1;i<=n;++i){
		while(he+1<=ta&&xl*sum[i]<rate(q[he+1],q[he]))	++he;
		dp[i]=calc(q[he])-xl*sum[i]*sum[q[he]]+1ll*a*sum[i]*sum[i]+1ll*b*sum[i]+c;
		while(he+1<=ta&&rate(q[ta],q[ta-1])<rate(i,q[ta]))	--ta;
		q[++ta]=i;
	}
	printf("%lld",dp[n]);
	return 0;
}
```