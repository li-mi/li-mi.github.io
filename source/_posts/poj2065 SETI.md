---
title: poj2065 SETI
tag:
 - 高斯消元
---
```
高斯消元解决模线性方程组
模数为质数且均相同
不同的不会，不是质数的更不会
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
const int N=78;
int T;
int p;
int a[N][N];
char s[N];
int n;
inline int ksm(int x,int y)
{
	int tmp=1;
	while(y){
		if(y&1)	tmp=tmp*x%p;
		x=x*x%p;
		y>>=1;
	}
	return tmp;
}
inline int inv(int x)
{
	return ksm(x,p-2);
}
inline void gauss()
{
	register int i,j,k,t1,t2;
	for(int j=1;j<=n;++j){
		if(a[j][j]==0){
			for(i=j+1;i<=n&&a[i][j]==0;++i);
			for(k=j;k<=n+1;++k){
				swap(a[i][k],a[j][k]);
			}
		}
		t1=inv(a[j][j]);
		for(i=1;i<=n;++i){
			if(i!=j&&a[i][j]){
				t2=a[i][j]*t1%p;
				for(k=j;k<=n+1;++k){
					a[i][k]=((a[i][k]-t2*a[j][k])%p+p)%p;
				}
			}
		}
	}
	for(int i=1;i<=n;++i){
		a[i][n+1]=a[i][n+1]*inv(a[i][i])%p;
	}
}
int main()
{
	T=read();
	while(T--){
		//memset(a,0,sizeof(a));
		p=read();
		scanf("%s",s+1);
		n=strlen(s+1);
		for(int i=1;i<=n;++i){
			if(s[i]!='*')	a[i][n+1]=s[i]-'a'+1;
			else	a[i][n+1]=0;
			a[i][1]=1;
			for(int j=2;j<=n;++j){
				a[i][j]=a[i][j-1]*i%p;
			}
		}
		gauss();
		for(int i=1;i<=n;++i){
			printf("%d ",a[i][n+1]);
		}
		puts("");
	}
	return 0;
}
```