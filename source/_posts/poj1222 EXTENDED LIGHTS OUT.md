---
title: poj1222 EXTENDED LIGHTS OUT
tag:
 - 高斯消元
---
```
经典的异或方程组
bitset用printf输出时要强制转换类型
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
int T;
int tmp;
bitset<38> a[38];
inline int getid(int x,int y){
	return (x-1)*6+y;
}
inline void gauss()
{
	register int i,j,k,now=1;
	for(i=1;i<=30;++i){
		if(a[now][i]==0){
			for(j=now+1;j<=30&&a[j][i]==0;++j);
			if(j>30)	continue;
			swap(a[now],a[j]);
		}
		for(k=1;k<=30;++k){
			if(k!=now&&a[k][i])	a[k]^=a[now];
		}
		++now;
	}
}
int main()
{
	T=read();
	for(int cas=1;cas<=T;++cas){
		printf("PUZZLE #%d\n",cas);
		for(int i=1;i<=30;++i){
			a[i].reset();
		}
		for(int i=1;i<=5;++i){
			for(int j=1;j<=6;++j){
				tmp=getid(i,j);
				a[tmp][31]=read();
				a[tmp][tmp]=1;
				if(i!=1){
					a[tmp][tmp-6]=1;
				}
				if(i!=5){
					a[tmp][tmp+6]=1;
				}
				if(j!=1){
					a[tmp][tmp-1]=1;
				}
				if(j!=6){
					a[tmp][tmp+1]=1;
				}
			}
		}
		gauss();
		for(int i=1;i<=5;++i){
			for(int j=1;j<=6;++j){
				printf("%d ",int(a[getid(i,j)][31]));
			}
			puts("");
		}
	}
	return 0;
}
```