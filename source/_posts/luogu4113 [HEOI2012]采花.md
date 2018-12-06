---
title: luogu4113 [HEOI2012]采花
tag: 树状数组
---
颓废好久
开始newtrain之后每道题都要写题解！！
区间限制数量是经典问题，向前连边即可
此题是求区间出现次数大于1的数的个数
两种求法：
1.对pre+1处-1,1处+1，query为左端点及其左的和，相当于对区间赋值，要撤回操作。
2.对pre处+1，pre[pre]处-1，query为右端点-（左端点-1），相当于统计每个点对区间的贡献，隐藏了撤回。
犯了一个小错误:把n和qn弄错了
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
const int N=2000008;
int n,c,qn;
int a[N];
struct que{
	int l,r,id;
}q[N];
inline bool cmp(const que &a,const que &b)
{
	return a.r<b.r;
}
int pre[N],pos[N];
int t;
int bit[N],ans[N];
inline void add(int pos,int x)
{
	for(int i=pos;i<=n;i+=i&-i){
		bit[i]+=x;
	}
}
inline int query(int pos)
{
	int tmp=0;
	for(int i=pos;i>0;i-=i&-i){
		tmp+=bit[i];
	}
	return tmp;
}
inline void solve(int pos,int type)
{
	if(pre[pos]>0){
		add(pre[pos]+1,-1*type);
		add(1,1*type);
	}
}
int main()
{
	n=read();c=read();qn=read();
	for(int i=1;i<=n;++i){
		a[i]=read();
	}
	for(int i=1;i<=n;++i){
		pre[i]=pos[a[i]];
		pos[a[i]]=i;
	}
	for(int i=1;i<=qn;++i){
		q[i].l=read();
		q[i].r=read();
		q[i].id=i;
	}
	sort(q+1,q+qn+1,cmp);
	t=1;
	for(int i=1;i<=n;++i){
		/*while(t<q[i].r){
			++t;
			solve(t,1);
			solve(pre[t],-1);
		}
		ans[q[i].id]=query(q[i].l); */
		if(pre[i]>0){
			add(pre[i],1);
			if(pre[pre[i]]>0)	add(pre[pre[i]],-1);
		}
		while(q[t].r==i&&t<=qn){
			ans[q[t].id]=query(q[t].r)-query(q[t].l-1);
			++t;
		}
	}
	for(int i=1;i<=qn;++i){
		printf("%d\n",ans[i]);
	}
	return 0;
}
```