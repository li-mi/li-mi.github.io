---
title: luogu1198 [JSOI2008]最大数
tag: 
 - 单调队列
 - 并查集
 - 树状数组
 - 线段树
 - 分块
---
```
做法很多
把板子复习一下
1.单调队列，然后二分
2.单调队列，然后维护每个位置的更优解，加上路径压缩
3.树状数组，需要从后往前加，最大值不能作减法
4.线段树
5.分块
```
<!--more-->
```
#include<bits/stdc++.h>
#define mp make_pair
#define pb push_back
using namespace std;
typedef long long LL;
typedef pair<int,int> PII;
const int M=200008;
int m,d,a;
char c[1];
int t,l;
int q[M],ta,tim[M];
int main()
{
	scanf("%d%d",&m,&d);
	for(int i=1;i<=m;++i){
		scanf("%s%d",c,&a);
		if(c[0]=='A'){
			a=(0ll+a+t)%d;
			while(ta&&q[ta]<=a)	--ta;
			q[++ta]=a;
			tim[ta]=++l; 
		}
		else{
			if(a){
				printf("%d\n",t=q[lower_bound(tim+1,tim+ta+1,l-a+1)-tim]);
			}
			else{
				puts("0");
			}
		}
	}
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
const int M=200008;
int m,d,a;
char c[1];
int t;
int q[M],ta,fa[M],num[M],id[M],len;
inline int find(int x)
{
	return x==fa[x]?x:fa[x]=find(fa[x]);
}
int main()
{
	scanf("%d%d",&m,&d);
	for(int i=1;i<=m;++i){
		scanf("%s%d",c,&a);
		if(c[0]=='A'){
			a=(0ll+a+t)%d;
			++len;
			num[len]=a;
			fa[len]=len;
			while(ta&&q[ta]<=a){
				fa[id[ta]]=len;
				--ta;
			}	
			q[++ta]=a;
			id[ta]=len;
		}
		else{
			if(a){
				printf("%d\n",t=num[find(len-a+1)]);
			}
			else{
				puts("0");
			}
		}
	}
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
const int M=200008;
int m,d,a;
char c[1];
int t,num,b[M];
inline void add(int p,int x)
{
	for(int i=p;i<=m;i+=i&-i){
		b[i]=max(b[i],x);
	}
}
inline int query(int p)
{
	int tmp=INT_MIN;
	for(int i=p;i;i-=i&-i){
		tmp=max(tmp,b[i]);
	}
	return tmp;
}
int main()
{
	scanf("%d%d",&m,&d);
	num=m;
	for(int i=1;i<=m;++i){
		b[i]=INT_MIN;
	}
	for(int i=1;i<=m;++i){
		scanf("%s%d",c,&a);
		if(c[0]=='A'){
			a=(0ll+a+t)%d;
			add(num,a);
			--num;
		}
		else{
			if(a){
				printf("%d\n",t=query(num+a));
			}
			else{
				puts("0");
			}
		}
	}
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
const int M=200008;
int m,d,a;
char c[1];
int len,t;
int tree[M<<2];
inline void update(int x,int l,int r)
{
	if(l==len&&r==len){
		tree[x]=a;
		return;
	}
	int mid=(l+r)>>1;
	if(len<=mid)	update(x<<1,l,mid);
	else	update(x<<1|1,mid+1,r);
	tree[x]=max(tree[x<<1],tree[x<<1|1]);
}
inline int query(int x,int l,int r)
{
	if(len-a+1<=l&&r<=len){
		return tree[x];
	}
	int mid=(l+r)>>1;
	if(len<=mid)	return query(x<<1,l,mid);
	if(len-a+1>mid)	return query(x<<1|1,mid+1,r);
	return max(query(x<<1,l,mid),query(x<<1|1,mid+1,r));
}
int main()
{
	scanf("%d%d",&m,&d);
	for(int i=1;i<=m;++i){
		scanf("%s%d",c,&a);
		if(c[0]=='A'){
			a=(0ll+a+t)%d;
			++len;
			update(1,1,m);
		}
		else{
			if(a){
				printf("%d\n",t=query(1,1,m));
			}
			else{
				puts("0");
			}
		}
	}
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
const int M=200008;
int m,d,a,t;
char c[1];
int blk,len;
int num[M],bl[M],maxi[M];
inline void update()
{
	++len;
	bl[len]=(len-1)/blk+1;
	num[len]=a;
	maxi[bl[len]]=max(maxi[bl[len]],a);
}
inline int query(int l,int r)
{
	int tmp=INT_MIN;
	for(int i=min(r,bl[l]*blk);i>=l;--i){
		tmp=max(tmp,num[i]);
	}
	for(int i=bl[l]+1;i<bl[r];++i){
		tmp=max(tmp,maxi[i]);
	}
	for(int i=max(l,(bl[r]-1)*blk+1);i<=r;++i){
		tmp=max(tmp,num[i]);
	}
	return tmp;
}
int main()
{
	scanf("%d%d",&m,&d);
	blk=sqrt(m);
	for(int i=m/blk+1;i;--i){
		maxi[i]=INT_MIN;
	}
	for(int i=1;i<=m;++i){
		scanf("%s%d",c,&a);
		if(c[0]=='A'){
			a=(0ll+a+t)%d;
			update();
		}
		else{
			if(a){
				printf("%d\n",t=query(len-a+1,len));
			}
			else{
				puts("0");
			}
		}
	}
	return 0;
}
```