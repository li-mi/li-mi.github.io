---
title: luogu2709 小B的询问
tag: 莫队
---
```
关于排序的速度：
cmp<重载<友元重载（部分数据即实验结果）

莫队裸题
小优化：
对奇数块，r从小到大
对偶数快，r从大到小
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
const int N=50008;
int n,m,k,blk;
int a[N];
int curl,curr;
int tmp,ans[N],cnt[N];
struct node{
	int l,r,id;
}q[N];
inline bool cmp(const node &a,const node &b)
{
	//return (a.l/blk==b.l/blk)?a.r<b.r:a.l<b.l;
	if(a.l/blk!=b.l/blk)	return a.l<b.l;
	if(a.l/blk%2)	return a.r<b.r;
	return a.r>b.r;
}
inline void add(int x)
{
	tmp+=2*cnt[a[x]]+1;
	++cnt[a[x]];
}
inline void remove(int x)
{
	tmp-=2*cnt[a[x]]-1;
	--cnt[a[x]];
}
int main()
{
	n=read();m=read();k=read();
	blk=sqrt(n);
	for(int i=1;i<=n;++i){
		a[i]=read();
	}
	for(int i=1;i<=m;++i){
		q[i].l=read();q[i].r=read();q[i].id=i;
	}
	sort(q+1,q+m+1,cmp);
	curl=1;curr=0;
	for(int i=1;i<=m;++i){
		while(curl>q[i].l){
			add(--curl);
		}
		while(curr<q[i].r){
			add(++curr);
		}
		while(curl<q[i].l){
			remove(curl);
			++curl;
		}
		while(curr>q[i].r){
			remove(curr);
			--curr;
		}
		ans[q[i].id]=tmp;
	}
	for(int i=1;i<=m;++i){
		printf("%d\n",ans[i]);
	}
	return 0;
}
```