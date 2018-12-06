---
title: poj2985The k-th Largest Group
tag: 
 - 树状数组
 - 二分
---
```
单点修改
找第k大
二分+树状数组，复杂度O(nlog^2n)
树状数组上二分，复杂度O(nlogn)
妙极了
树状数组二分和线段树二分差不多
```
<!--more-->
二分+树状数组
```
#include<cstdio>
#define mp make_pair
#define pb push_back
using namespace std;
typedef long long LL;
inline LL read()
{
	LL x=0,f=1;char ch=getchar();
	while(ch<'0'||ch>'9'){if(ch=='-')f=-1;ch=getchar();}
	while(ch>='0'&&ch<='9'){x=(x<<1)+(x<<3)+(ch^48);ch=getchar();}
	return x*f;
}
const int N=300008;
int n,m;
int op,x,y;
int p[N],p1,p2;
int a[N],bit[N];
int num;
int l,r,mid,k,ans;
inline void add(int pos,int x)
{
	for(int i=pos;i<=n;i+=i&-i){
		bit[i]+=x;
	}
}
inline int sum(int pos)
{
	int tmp=0;
	for(int i=pos;i;i-=i&-i){
		tmp+=bit[i];
	}
	return tmp;
}
inline int find(int x)
{
	return x==p[x]?x:p[x]=find(p[x]);
}
int main()
{
	n=read();m=read();
	for(int i=1;i<=n;++i){
		p[i]=i;
	}
	for(int i=1;i<=n;++i){
		a[i]=1;
	}
	add(1,n);
	num=n;
	while(m--){
		op=read();
		if(op==0){
			x=read();y=read();
			p1=find(x);
			p2=find(y);
			if(p1==p2)	continue;
			add(a[p1],-1);
			add(a[p2],-1);
			add(a[p2]=a[p1]+a[p2],1);
			p[p1]=p2;
			--num;
		}
		else{
			k=read();
			k=num-k+1;
			l=1;
			r=n;
			while(l<=r){
				mid=(l+r)>>1;
				if(sum(mid)>=k){
					r=mid-1;
					ans=mid;
				}
				else{
					l=mid+1;
				}
			}
			printf("%d\n",ans);
		}
	}
	return 0;
}
```
树状数组上二分
```
#include<cstdio>
#define mp make_pair
#define pb push_back
using namespace std;
typedef long long LL;
inline LL read()
{
	LL x=0,f=1;char ch=getchar();
	while(ch<'0'||ch>'9'){if(ch=='-')f=-1;ch=getchar();}
	while(ch>='0'&&ch<='9'){x=(x<<1)+(x<<3)+(ch^48);ch=getchar();}
	return x*f;
}
const int N=300008;
int n,m;
int op,x,y;
int p[N],p1,p2;
int a[N],bit[N];
int num;
int k;
inline void add(int pos,int x)
{
	for(int i=pos;i<=n;i+=i&-i){
		bit[i]+=x;
	}
}
inline int sum(int pos)
{
	int tmp=0;
	for(int i=pos;i;i-=i&-i){
		tmp+=bit[i];
	}
	return tmp;
}
inline int find(int x)
{
	return x==p[x]?x:p[x]=find(p[x]);
}
inline int query(int pos)
{
	int tmp=0,cnt=0;
	for(int i=20;i>=0;--i){
		tmp+=1<<i;
		if(tmp>=n||cnt+bit[tmp]>=pos){
			tmp-=1<<i;
		}
		else{
			cnt+=bit[tmp];
		}
	}
	return tmp+1;
}
int main()
{
	n=read();m=read();
	for(int i=1;i<=n;++i){
		p[i]=i;
	}
	for(int i=1;i<=n;++i){
		a[i]=1;
	}
	add(1,n);
	num=n;
	while(m--){
		op=read();
		if(op==0){
			x=read();y=read();
			p1=find(x);
			p2=find(y);
			if(p1==p2)	continue;
			add(a[p1],-1);
			add(a[p2],-1);
			add(a[p2]=a[p1]+a[p2],1);
			p[p1]=p2;
			--num;
		}
		else{
			k=read();
			printf("%d\n",query(num-k+1));
		}
	}
	return 0;
}
```