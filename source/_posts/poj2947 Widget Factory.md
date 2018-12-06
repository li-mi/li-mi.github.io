---
title: poj2947 Widget Factory
tag: 高斯消元
---
```
消成对角线的方法就不行了
不是方阵
消成行阶梯形
从now开始以后有系数全为0,但右边不为0的就无解
否则
now<n多解

这题数据挺大的（一般题都是0ms）
高斯约旦法在速度上的差异体现出来了
会慢100ms左右
```
<!--more-->
```
#include<iostream>
#include<cstdio>
#include<cstring>
#include<climits>
#include<queue>
#include<bitset>
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
const int mod=7,N=308,M=308;
int n,m,k;
char s[5],t[5];
int a[M][N];
int cas;
int inv[10];

inline int sol(char *x)
{
	if(x[0]=='M')	return 1;
	if(x[0]=='T'&&x[1]=='U')	return 2;
	if(x[0]=='W')	return 3;
	if(x[0]=='T'&&x[1]=='H')	return 4;
	if(x[0]=='F')	return 5;
	if(x[0]=='S'&&x[1]=='A')	return 6;
	if(x[0]=='S'&&x[1]=='U')	return 7;
}
inline int gauss()
{
	register int i,j,k,t,now=1;
	for(j=1;j<=n;++j){
		if(a[now][j]==0){
			for(i=now+1;i<=m&&a[i][j]==0;++i);
			if(i>m)	continue;
			for(k=j;k<=n+1;++k){
				swap(a[i][k],a[now][k]);
			}
		}
		for(i=1;i<=m;++i){
			if(i!=now&&a[i][j]){
				t=a[i][j]*inv[a[now][j]]%mod;
				for(k=j;k<=n+1;++k){
					a[i][k]=(a[i][k]-t*a[now][k]%mod+mod)%mod;
				}
			}
		}
		++now;
	}
	
	for(int i=now;i<=m;++i){
		if(a[i][n+1]){
			int f=0;
			for(int j=i;j<=n;++j){
				if(a[i][j]){
					f=1;
					break;
				}
			}
			if(!f)	return -1;//无解 
		}
	}
	
	if(now<n)	return 1;//有自由变量，多解 
	for(int i=1;i<=n;++i){
		a[i][n+1]=a[i][n+1]*inv[a[i][i]]%mod;
	}
	return 0;
}
int main()
{
	inv[1]=1;
	for(int i=2;i<mod;++i){
		inv[i]=mod-mod/i*inv[mod%i]%mod;
	}
	while(1){
		n=read();m=read();
		if(n==0&&m==0)	return 0;
		memset(a,0,sizeof(a));
		
		for(int i=1;i<=m;++i){
			k=read();
			scanf("%s%s",s,t);
			a[i][n+1]=(sol(t)-sol(s)+1+mod)%mod;
			while(k--)	++a[i][read()];
			for(int j=1;j<=n;++j)	a[i][j]%=mod;
		}
		
		cas=gauss();
		if(cas==-1){
			puts("Inconsistent data.");
		}
		else if(cas==1){
			puts("Multiple solutions.");
		}
		else{
			for(int i=1;i<=n;++i){
				printf("%d ",a[i][n+1]<3?a[i][n+1]+mod:a[i][n+1]);
			}
			puts(" ");
		}
	}
	return 0;
}
```
```
#include<iostream>
#include<cstdio>
#include<cstring>
#include<climits>
#include<queue>
#include<bitset>
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
const int mod=7,N=308,M=308;
int n,m,k;
char s[5],t[5];
int a[M][N];
int cas;
int inv[10];

inline int sol(char *x)
{
	if(x[0]=='M')	return 1;
	if(x[0]=='T'&&x[1]=='U')	return 2;
	if(x[0]=='W')	return 3;
	if(x[0]=='T'&&x[1]=='H')	return 4;
	if(x[0]=='F')	return 5;
	if(x[0]=='S'&&x[1]=='A')	return 6;
	if(x[0]=='S'&&x[1]=='U')	return 7;
}
inline int gauss()
{
	register int i,j,k,t,now=1;
	for(j=1;j<=n;++j){
		if(a[now][j]==0){
			for(i=now+1;i<=m&&a[i][j]==0;++i);
			if(i>m)	continue;
			for(k=j;k<=n+1;++k){
				swap(a[i][k],a[now][k]);
			}
		}
		for(i=now+1;i<=m;++i){
			if(a[i][j]){
				t=a[i][j]*inv[a[now][j]]%mod;
				for(k=j;k<=n+1;++k){
					a[i][k]=(a[i][k]-t*a[now][k]%mod+mod)%mod;
				}
			}
		}
		++now;
	}
	
	for(i=now;i<=m;++i){
		if(a[i][n+1]){
			int f=0;
			for(j=i;j<=n;++j){
				if(a[i][j]){
					f=1;
					break;
				}
			}
			if(!f)	return -1;//ÎÞ½â 
		}
	}
	
	if(now<n)	return 1;//ÓÐ×ÔÓÉ±äÁ¿£¬¶à½â 
	for(i=n;i;--i){
		for(j=n;j>i;--j){
			a[i][n+1]=(a[i][n+1]-a[i][j]*a[j][n+1]%mod+mod)%mod;
		}
		a[i][n+1]=a[i][n+1]*inv[a[i][i]]%mod;
	}
	return 0;
}
int main()
{
	inv[1]=1;
	for(int i=2;i<mod;++i){
		inv[i]=mod-mod/i*inv[mod%i]%mod;
	}
	while(1){
		n=read();m=read();
		if(n==0&&m==0)	return 0;
		memset(a,0,sizeof(a));
		
		for(int i=1;i<=m;++i){
			k=read();
			scanf("%s%s",s,t);
			a[i][n+1]=(sol(t)-sol(s)+1+mod)%mod;
			while(k--)	++a[i][read()];
			for(int j=1;j<=n;++j)	a[i][j]%=mod;
		}
		
		cas=gauss();
		if(cas==-1){
			puts("Inconsistent data.");
		}
		else if(cas==1){
			puts("Multiple solutions.");
		}
		else{
			for(int i=1;i<=n;++i){
				printf("%d ",a[i][n+1]<3?a[i][n+1]+mod:a[i][n+1]);
			}
			puts(" ");
		}
	}
	return 0;
}
```