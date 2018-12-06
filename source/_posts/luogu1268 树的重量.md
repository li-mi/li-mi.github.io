---
title: luogu1268 树的重量
tag:
 - 思维
---
```
好题啊
看了半天不会
去看题解
妙啊
还是不明白
重点就是不知道为什么树的重量是一定的
而做法就相当于证明一样
每加上去一个点，相当于从一对点的路径上伸出来一条边

看题解时发现一个有趣的结论
根据两两点之间的最短距离建出的树应该都是同构的（边权均非负）
然而应该不是同构
每次选最小的值w(i,j)
若存在k，使得w(i,k)+w(k,j)=w(i,j)，则i,j之间不需要连边
否则就连边
这样应该可以确定一个边数最少的图
在这个图上填一写较大的边（大于两端点的最短距离）不会产生任何影响
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
const int N=38;
int n;
int a[N][N];
int ans,tmp;
int main()
{
	while(1){
		n=read();
		if(!n)	return 0;
		memset(a,0,sizeof(a));
		for(int i=1;i<n;++i){
			for(int j=i+1;j<=n;++j){
				a[i][j]=read();
				a[j][i]=a[i][j];
			}
		}
		ans=a[1][2];
		for(int i=3;i<=n;++i){
			tmp=INT_MAX;
			for(int j=1;j<i;++j){
				for(int k=j+1;k<i;++k){
					tmp=min(tmp,(a[j][i]+a[k][i]-a[j][k])/2);
				}
			}
			ans+=tmp;
		}
		printf("%d\n",ans);
	}
	return 0;
}
```