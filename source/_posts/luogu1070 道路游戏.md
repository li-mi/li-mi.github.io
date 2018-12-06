---
title: luogu1070 道路游戏
tag: 
 - dp
 - 单调队列
---
```
dp
f[i]表示到i时刻的最大收益
sum[k][j]表示第k个机器人前j时间的收益
f[i]=max(f[j]+sum[k][i]-sum[k][j]-a[getid(k+j)];
其中a[]的下标应该是从k往后走j步的地方
记得把f[]初始化成最小的，答案可能是负的
这样O($n^3$)就能过了（毕竟pj题）

2d/1d考虑优化成2d/0d
单调队列
i不好放到单调队列里
j有限制（j>=i-p），j应该是单调队列的一个下标
然而一维并不好处理这个问题
q[k][j]表示对每个k的一个单调队列
max(f[j]-sum[k][j]-a[getid(k+j)]->q[k][j]
这样对每个i只需要枚举k
跑了100+ms

然而candy?的代码20+ms
但他的代码中有一点问题
过不了hack
f[i][j]中最大不一定是最好（有后效性）
因为step[i][j]=p时，是无法向后转移的
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
const int N=1008;
int n,m,p;
int a[N],b[N][N],sum[N][N],f[N];
inline int getid(int x)
{
	return (x-1)%n+1;
}
int main()
{
	n=read();m=read();p=read();
	for(int i=1;i<=n;++i){
		for(int j=1;j<=m;++j){
			b[i][j]=read();
		}
	}
	for(int i=1;i<=n;++i){
		for(int j=1;j<=m;++j){
			sum[i][j]=sum[i][j-1]+b[getid(i+j-1)][j];
		}
	}
	for(int i=1;i<=n;++i){
		a[i]=read();
	}
	for(int i=1;i<=m;++i){
		f[i]=INT_MIN;
	}
	for(int i=1;i<=m;++i){
		for(int j=i-1;j>=i-p&&j>=0;--j){
			for(int k=1;k<=n;++k){
				f[i]=max(f[i],f[j]+sum[k][i]-sum[k][j]-a[getid(k+j)]);
			}
		}
	}
	printf("%d",f[m]);
	return 0;
}
```
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
const int N=1008;
int n,m,p;
int a[N],b[N][N],sum[N][N],f[N];
int tmp;
PII q[N][N];
int he[N],ta[N];
inline int getid(int x)
{
	return (x-1)%n+1;
}
int main()
{
	n=read();m=read();p=read();
	for(int i=1;i<=n;++i){
		for(int j=1;j<=m;++j){
			b[i][j]=read();
		}
	}
	for(int i=1;i<=n;++i){
		for(int j=1;j<=m;++j){
			sum[i][j]=sum[i][j-1]+b[getid(i+j-1)][j];
		}
	}
	for(int i=1;i<=n;++i){
		a[i]=read();
	}
	for(int i=1;i<=m;++i){
		f[i]=INT_MIN;
	}
	for(int i=1;i<=m;++i){
		for(int k=1;k<=n;++k){
			tmp=f[i-1]-sum[k][i-1]-a[getid(k+i-1)];
			while(tmp>q[k][ta[k]].first&&ta[k]>he[k]){
				--ta[k];
			}
			q[k][++ta[k]]=mp(tmp,i-1);
			
		}
		for(int k=1;k<=n;++k){
			while(q[k][he[k]+1].second<i-p){
				++he[k];
			}
			f[i]=max(f[i],q[k][he[k]+1].first+sum[k][i]);
		}
	}
	printf("%d",f[m]);
	return 0;
}
```