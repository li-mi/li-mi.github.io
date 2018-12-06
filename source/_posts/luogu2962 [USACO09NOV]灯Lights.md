---
title: luogu2962 [USACO09NOV]灯Lights
tag:
 - 高斯消元
 - MITM
---
```
高斯消元
然后搜索
枚举每个自由变量的值
为了方便确定自由变量的位置
a[i][i]=0表示i是自由变量
也就是将系数矩阵消成对角线
若对角线上为1，则1上面为0
若对角线上为0，则0上面可能有1
也就是是说
高斯消元不必消成行阶梯形
（a[i][j]==0写成a[i][j]，小错误要避免）

meet in the middle也能做
比高斯消元慢一点（不过感觉搜索的复杂度更没有保证啊。。）
代码抄hzwer
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
const int N=40;
int n,m;
bitset<N> a[N];
int x,y,now;
int cnt,ans=INT_MAX,val[N];
inline void gauss()
{
	register int i,j;
	for(int j=1;j<=n;++j){
		if(a[j][j]==0){
			for(i=j+1;i<=n&&a[i][j]==0;++i);
			if(i>n)	continue;
			swap(a[j],a[i]);
		}
		for(int i=1;i<=n;++i){
			if(i!=j&&a[i][j]){
				a[i]^=a[j];
			}
		}
	}
}
inline void dfs(int x)
{
	if(cnt>=ans)	return;
	if(!x){
		ans=cnt;
		return;
	}
	if(a[x][x]){
		int t=a[x][n+1];
		for(int j=x+1;j<=n;++j){
			if(a[x][j])	t^=val[j];
		}
		val[x]=t;
		if(t){
			++cnt;
			dfs(x-1);
			--cnt;
		}
		else{
			dfs(x-1);
		}
	}
	else{
		val[x]=0;
		dfs(x-1);
		val[x]=1;
		++cnt;
		dfs(x-1);
		--cnt;
	}
}
int main()
{
	n=read();m=read();
	for(int i=1;i<=n;++i){
		a[i][i]=a[i][n+1]=1;
	}
	for(int i=1;i<=m;++i){
		x=read();y=read();
		a[x][y]=a[y][x]=1;
	}
	
	gauss();
	dfs(n);
	printf("%d",ans);
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
const int N=40;
int n,m;
int x,y;
LL ub,a[N];
int cnt,ans=INT_MAX;
map<LL,int> num;
inline void dfs(int x,int tmp,LL key)
{
	if(x>cnt){
		if(key==ub){
			ans=min(ans,tmp);
			return;
		}
		if(cnt==n){
			int t=num[ub-key];
			if(t)	ans=min(ans,t+tmp);
		}
		else{
			int t=num[key];
			if(!t||tmp<t)	num[key]=tmp;
		}
		return;
	}
	dfs(x+1,tmp,key);
	dfs(x+1,tmp+1,key^a[x]);
}
int main()
{
	n=read();m=read();
	ub=(1ll<<n)-1;
	for(int i=1;i<=n;++i){
		a[i]=1ll<<(i-1);
	}
	for(int i=1;i<=m;++i){
		x=read();y=read();
		a[x]+=1ll<<(y-1);
		a[y]+=1ll<<(x-1);
	}
	cnt=n/2;
	dfs(1,0,0);
	cnt=n;
	dfs(n/2+1,0,0);
	printf("%d",ans);
	return 0;
}
```