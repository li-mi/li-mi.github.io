---
title: luogu1415 拆分数列
tag: dp
---
```
f[i]表示从前往后到i时，最后一个数为f[i]-i（a-b表示从第a位到第b位），满足数列严格递增的f[i]的最大值
g[i]表示从后往前到i时，最后一个数为i-g[i]，满足数列严格递增的g[i]的最大值
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
const int N=508;
char a[N];
int n;
int f[N],g[N];
int pos=1;
inline bool cmp(int l1,int r1,int l2,int r2)
{
	while(l1<r1&&a[l1]=='0')	++l1;
	while(l2<r2&&a[l2]=='0')	++l2;
	if(r1-l1<r2-l2)	return 1;
	if(r1-l1>r2-l2)	return 0;
	int len=r1-l1;
	for(int i=0;i<=len;++i){
		if(a[l1+i]<a[l2+i])	return 1;
		if(a[l1+i]>a[l2+i])	return 0;
	}
	return 0;
}
inline void dp1()
{
	for(int i=1;i<=n;++i){
		f[i]=1;
		for(int j=i;j>1;--j){
			if(cmp(f[j-1],j-1,j,i)){
				f[i]=j;
				break;
			}
		}
	}
}
inline void dp2()
{
	int tmp=f[n];
	g[tmp]=n; 
	while(a[tmp-1]=='0')	g[--tmp]=n;
	for(int i=tmp-1;i;--i){
		for(int j=f[n]-1;j>=i;--j){
			if(cmp(i,j,j+1,g[j+1])){
				g[i]=j;
				break;
			}
		}
	}
}
int main()
{
	scanf("%s",a+1);
	n=strlen(a+1);
	dp1();
	dp2();
	while(1){
		for(int i=pos;i<=g[pos];++i){
			printf("%c",a[i]);
		}
		pos=g[pos]+1;
		if(pos<=n){
			printf(",");
		}
		else{
			break;
		}
	}
	return 0;
}
```