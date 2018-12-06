---
title: luogu3389 【模板】高斯消元法
tag:
 - 模板
 - 高斯消元
---
```
k逆序枚举可以减少中间变量，提高精度
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
const int N=108;
const double eps=1e-8;
int n;
double a[N][N];
int tmp;
inline bool gauss()
{
	for(int i=1;i<=n;++i){
		tmp=i;
		for(int j=i+1;j<=n;++j){
			if(fabs(a[j][i])>fabs(a[tmp][i])){
				tmp=j;
			}
		}
		if(tmp!=i){
			for(int j=i;j<=n+1;++j){
				swap(a[tmp][j],a[i][j]);
			}
		}
		if(fabs(a[i][i])<eps)	return 0;
		for(int j=i+1;j<=n;++j){
			for(int k=n+1;k>=i;--k){
				a[j][k]-=a[j][i]/a[i][i]*a[i][k];
			}
		}
	}
	for(int i=n;i;--i){
		for(int j=i+1;j<=n;++j){
			a[i][n+1]-=a[i][j]*a[j][n+1];
		}
		a[i][n+1]/=a[i][i];
	}
	return 1;
}
int main()
{
	n=read();
	for(int i=1;i<=n;++i){
		for(int j=1;j<=n;++j){
			a[i][j]=read();
		}
		a[i][n+1]=read();
	}
	if(gauss()){
		for(int i=1;i<=n;++i){
			printf("%.2lf\n",a[i][n+1]);
		}
	}
	else{
		puts("No Solution");
	}
	return 0;
}
```