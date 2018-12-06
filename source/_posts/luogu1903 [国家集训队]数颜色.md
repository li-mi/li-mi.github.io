---
title: luogu1903 [国家集训队]数颜色
tag: 莫队
---
```
带修改的莫队
设块长为blk
则复杂度为O(n*blk+n*n/blk+(n/blk)*(n/blk)*n)
均值求出blk=n^(2/3)
总复杂度为O(n^5/3)

排序的时候一定要求出每个点所属的块
不要用除法！
不要用除法！
不要用除法！
（t到死）
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
const int N=50008,MAX=1000008;
int n,m,blk;
int a[N],b[N],be[N];
char ch;
int idxq,idxc;
int curl,curr,curt;
int tmp,ans[N],cnt[MAX];
struct node{
	int l,r,tim,id;
}q[N];
struct node2{
	int pos,now,pre;
}c[N];
inline bool cmp(const node &a,const node &b)
{
	return (be[a.l]==be[b.l])?((be[a.r]==be[b.r])?a.tim<b.tim:a.r<b.r):a.l<b.l;
}
inline void add(int x)
{
	if(!cnt[x])	++tmp;
	++cnt[x];
}
inline void remove(int x)
{
	--cnt[x];
	if(!cnt[x])	--tmp;
}
inline void going(int pos,int x)
{
	if(curl<=pos&&pos<=curr){
		remove(a[pos]);
		add(x);
	}
	a[pos]=x;
}
int main()
{
	n=read();m=read();
	blk=pow(n,0.666666);
	for(int i=1;i<=n;++i){
		b[i]=a[i]=read();
		be[i]=i/blk+1;
	}
	for(int i=1;i<=m;++i){
		ch=getchar();
		while(ch!='Q'&&ch!='R')	ch=getchar();
		if(ch=='Q'){
			q[++idxq].l=read();
			q[idxq].r=read();
			q[idxq].tim=idxc;
			q[idxq].id=idxq;
		}
		else{
			c[++idxc].pos=read();
			c[idxc].pre=b[c[idxc].pos];
			b[c[idxc].pos]=c[idxc].now=read();
		}
	}
	sort(q+1,q+idxq,cmp);
	for(int i=1;i<=idxq;++i){
		while(curt<q[i].tim){
			++curt;
			going(c[curt].pos,c[curt].now);
		} 
		while(curt>q[i].tim){
			going(c[curt].pos,c[curt].pre);
			--curt;
		}
		while(curl>q[i].l){
			add(a[--curl]);
		}
		while(curr<q[i].r){
			add(a[++curr]);
		}
		while(curl<q[i].l){
			remove(a[curl]);
			++curl;
		}
		while(curr>q[i].r){
			remove(a[curr]);
			--curr;
		}
		ans[q[i].id]=tmp;
	}
	for(int i=1;i<=idxq;++i){
		printf("%d\n",ans[i]);
	} 
	return 0;
}
```