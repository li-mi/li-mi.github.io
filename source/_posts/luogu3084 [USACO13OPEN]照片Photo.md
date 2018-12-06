---
title: luogu3084 [USACO13OPEN]照片Photo
tag:
- dp
- 差分约束
---
```
区间中有多少个数的题目
和pku的夏令营题目挺像的
这种题目好像都可以用差分约束做
不过卡时
这题spfa只能过6个点
d[i]表示从1到i的个数
d[i]和d[i+1]的关系为
d[i]=d[i+1]或d[i]+1=d[i+1]
拆成不等式为
d[i]=d[i+1] => d[i]<=d[i+1] d[i]>=d[i+1]
d[i]+1=d[i+1] => d[i]<=d[i+1]-1 d[i]+1>=d[i+1]
合并为下面两个（若a<=b，则要让b尽量大）
d[i]<=d[i+1]
d[i]+1>=d[i+1]
初始情况为d[0]=0，其他无穷大
（
松弛大于n次就有负环一点都不靠谱，随便构造出反例
入队大于n次才是正确的，因为
假如没有负环，bfs一层就能至少确定一个点的dis
所以最多做n(其实n-1，但是要考虑1个点)轮，>n就无解(其实>=n)
每个点在每一层最多入队1次(可能被松弛很多次)
所以入队次数大于n就无解
）

正解dp
f[i]表示前i个已经满足条件，第i个放了的最大个数
f[i]=max(f[j])+1
j的范围是关键
设i对应的j满足l[i]<=j<=r[i]
有且只有1个这个条件拆成
至少1个和至多1个
l[i]为区间右端点在i左边的左端点的最大值（因为至少1个）
r[i]为i所在的区间的左端点的最小值-1（因为至多1个）
注意到l[i]和r[i]都是单调不降的
所以转移可以用单调队列优化
求l[i]和r[i]也可以利用单调性的特点做到O(n)
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
const int N=200008,M=100008;
int n,m;
struct node{
	int l,r;
}a[M];
int l[N],r[N];
int q[N],he,ta,t;
int f[N];
int main()
{
	n=read();m=read();
	for(int i=1;i<=n+1;++i){
		r[i]=i-1;
	}
	for(int i=1;i<=m;++i){
		a[i].l=read();a[i].r=read();
		l[a[i].r+1]=max(l[a[i].r+1],a[i].l);
		r[a[i].r]=min(r[a[i].r],a[i].l-1);
	}
	for(int i=1;i<=n;++i)	l[i+1]=max(l[i+1],l[i]);
	for(int i=n;i;--i)	r[i]=min(r[i],r[i+1]);
	he=ta=1;//f[0]=0,q[1]=0;
	t=1;
	for(int i=1;i<=n+1;++i){
		while(he<=ta&&q[he]<l[i])	++he;
		if(t<l[i])	t=l[i];
		while(t<=r[i]){
			if(f[t]!=-1){
				while(he<=ta&&f[t]>=f[q[ta]])	--ta;
				q[++ta]=t;
			}
			++t;
		}
		if(he<=ta){
			f[i]=f[q[he]]+1;
		}
		else{
			f[i]=-1;
		}
	} 
	if(f[n+1]==-1)	puts("-1");
	else	printf("%d",f[n+1]-1);
	return 0;
}
```
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
const int N=200008,M=100008;
int n,m;
int l,r;
int nume,head[N];
struct edge{
	int to,nxt,f;
}e[N*2+M*2];
inline void add_edge(int x,int y,int z)
{
	e[++nume]=(edge){y,head[x],z};head[x]=nume;
}
int dis[N];
bool inq[N];
int q[N],he,ta;
int tim[N]; 
inline void spfa()
{
	memset(dis,0x3f,sizeof(dis));
	dis[0]=0;
	he=1;ta=2;
	q[1]=0;
	inq[0]=1;
	tim[0]=1;
	int maxi=min(n,1000); 
	while(he!=ta){
		int x=q[he];
		++he;
		if(he==200005)	he=1;
		inq[x]=0;
		for(int i=head[x];i;i=e[i].nxt){
			if(dis[e[i].to]>dis[x]+e[i].f){
				dis[e[i].to]=dis[x]+e[i].f;
				tim[e[i].to]=tim[x]+1;
				if(tim[e[i].to]>maxi){
					puts("-1");
					return;
				}
				if(!inq[e[i].to]){
					q[ta]=e[i].to;
					++ta;
					if(ta==200005)	ta=1;
					inq[e[i].to]=1;
				}
			}
		}
	}
	printf("%d",dis[n]);
	return;
}
int main()
{
	n=read();m=read();
	for(int i=1;i<=m;++i){
		l=read();r=read();
		add_edge(l-1,r,1);
		add_edge(r,l-1,-1);
	}
	for(int i=0;i<n;++i){
		add_edge(i,i+1,1);
		add_edge(i+1,i,0);
	}
	spfa();
	return 0;
}
```