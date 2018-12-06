---
title: luogu3989 [SHOI2013]阶乘字符串（序列自动机）
tag: 
- 序列自动机
- 状压
- dp
---
```
序列自动机就是dp。。
记录next[i][26]，O(26n)的预处理，从n到1，每次a[26]记录最后出现的位置，然后复制给next就可以了（next是dag）
状压加手算优化（大于21就无解我也不会证）
可以看第一道魔改题
构造出n^2-2n+4的数列（所以可以卡22）
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
const int N=28,L=458;
int T;
int n,len;
char s[L];
int next[L][N];
inline void get_next()
{
	for(int i=0;i<n;++i){
		next[len][i]=next[len+1][i]=len+1;
	}
	for(int i=len;i;--i){
		for(int j=0;j<n;++j){
			next[i-1][j]=next[i][j];
		}
		next[i-1][s[i]-'a']=i;
	}
}
int f[1<<21];
inline bool dp()
{
	for(int i=1;i<(1<<n);++i){
		f[i]=0;
		//for(int j=0;j<n;++j){
		//	if((i>>j)&1){
		//		f[i]=max(f[i],next[f[i^(1<<j)]][j]);
		//	}
		//}
		for(int j=i;j;j^=j&-j){
			f[i]=max(f[i],next[f[i^(j&-j)]][__builtin_ctz(j)]);
		}
		if(f[i]>len)	return 0;
	}
	return 1;
}
int main()
{
	T=read();
	while(T--){
		n=read();
		scanf("%s",s+1);
		if(n>21){
			puts("NO");
			continue;
		}
		len=strlen(s+1);
		get_next();
		if(dp())	puts("YES");
		else	puts("NO");
	}
	return 0;
}
```