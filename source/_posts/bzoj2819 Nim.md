---
title: bzoj2819 Nim
tag:
- 树链剖分
- 线段树
- 树状数组
- dfs序
---
裸树剖+线段树
码码码
挂了
查了好久
发现dfs2挂了
伤心欲绝以为我一直以来的板子都是错的
开始吐槽过去a掉的题目数据水
然后发现
只有套线段树才会挂
因为没有先dfs重孩子（原来我写的都是求lca吗。

正解是在dfs序上搞
发现答案是两点到根的异或和再异或lca
一个点只对子树有贡献
然后
我跑去写手写栈
其实是第一次写
入栈的时候处理一个点的信息
全部处理完出栈
出栈时处理对父亲的贡献
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
const int N=500008;
int n,qn;
int a[N];
int x,y;
vector<int> e[N];
int sz[N],dep[N],fa[N],hvson[N],tf[N],id[N],w[N],num;
inline void dfs1(int x)
{
	sz[x]=1;
	for(int i=0;i<e[x].size();++i){
		if(e[x][i]!=fa[x]){
			fa[e[x][i]]=x;
			dep[e[x][i]]=dep[x]+1;
			dfs1(e[x][i]);
			sz[x]+=sz[e[x][i]];
			if(sz[hvson[x]]<sz[e[x][i]])	hvson[x]=e[x][i];
		}
	}
}
inline void dfs2(int x)
{
	id[x]=++num;
	w[num]=a[x];
	//要先dfs重孩子！！ 
	/*tf[x]=(hvson[fa[x]]==x?tf[fa[x]]:x);
	for(int i=0;i<e[x].size();++i){
		if(e[x][i]!=fa[x])	dfs2(e[x][i]);
	}*/
	/*tf[x]=ances;
	if(!hvson[x])	return;
	dfs2(hvson[x],ances);
	for(int i=0;i<e[x].size();++i){
		if(e[x][i]!=fa[x]&&e[x][i]!=hvson[x])	dfs2(e[x][i],e[x][i]);
	}*/
	tf[x]=(hvson[fa[x]]==x?tf[fa[x]]:x);
	if(!hvson[x])	return;
	dfs2(hvson[x]);
	for(int i=0;i<e[x].size();++i){
		if(e[x][i]!=fa[x]&&e[x][i]!=hvson[x])	dfs2(e[x][i]);
	}
}
int tr[N<<2];
inline void pushup(int t)
{
	tr[t]=tr[t<<1]^tr[t<<1|1];
}
inline void build_segment_tree(int t,int l,int r)
{
	if(l==r){
		tr[t]=w[l];
		return;
	}
	int mid=(l+r)>>1;
	build_segment_tree(t<<1,l,mid);
	build_segment_tree(t<<1|1,mid+1,r);
	pushup(t);
}
inline void update(int t,int l,int r,int pos,int val)
{
	if(l==r){
		tr[t]=val;
		return;
	}
	int mid=(l+r)>>1;
	if(pos<=mid)	update(t<<1,l,mid,pos,val);
	else	update(t<<1|1,mid+1,r,pos,val);
	pushup(t);
}
inline int query(int t,int l,int r,int ll,int rr)
{
	if(ll<=l&&r<=rr){
		return tr[t];
	}
	int mid=(l+r)>>1;
	if(rr<=mid)	return query(t<<1,l,mid,ll,rr);
	if(ll>mid)	return query(t<<1|1,mid+1,r,ll,rr);
	return query(t<<1,l,mid,ll,rr)^query(t<<1|1,mid+1,r,ll,rr);
}
inline void solve(int x,int y)
{
	int ans=0;
	while(tf[x]!=tf[y]){
		if(dep[tf[x]]>dep[tf[y]]){
			ans^=query(1,1,n,id[tf[x]],id[x]);
			x=fa[tf[x]];
		}
		else{
			ans^=query(1,1,n,id[tf[y]],id[y]);
			y=fa[tf[y]];
		}
	}
	if(dep[x]<dep[y]){
		ans^=query(1,1,n,id[x],id[y]);
	}
	else{
		ans^=query(1,1,n,id[y],id[x]);
	}
	//下面是假做法，lca没有被考虑到 
	/*
	while(x>0){
		ans^=query(1,1,n,id[tf[x]],id[x]);
		x=fa[tf[x]];
	}
	while(y>0){
		ans^=query(1,1,n,id[tf[y]],id[y]);
		y=fa[tf[y]];
	}*/
	if(ans){
		puts("Yes");
	}
	else{
		puts("No");
	}
}
char op;
int main()
{
	n=read();
	for(int i=1;i<=n;++i){
		a[i]=read();
	}
	for(int i=1;i<n;++i){
		x=read();y=read();
		e[x].pb(y);
		e[y].pb(x);
	}
	
	dfs1(1);
	dfs2(1);
	build_segment_tree(1,1,n);
	qn=read();
	while(qn--){
		op=getchar();
		while(op!='Q'&&op!='C')	op=getchar();
		if(op=='Q'){
			x=read();y=read();
			solve(x,y);
		}
		else{
			x=read();y=read();
			update(1,1,n,id[x],y);
		}
	}
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
const int N=500008;
int n,qn;
int a[N];
int x,y,z;
vector<int> e[N];
int sta[N],ta;
int sz[N],fa[N],dep[N],hvson[N],tf[N];
int cur[N];
inline void dfs1()
{
	memset(cur,0,sizeof(cur)); 
	ta=0;
	sta[++ta]=1;
	sz[1]=1;
	while(ta>0){
		int x=sta[ta];
		int &i=cur[x];
		if(e[x][i]==fa[x])	++i;
		if(i>=e[x].size()){
			--ta;
			if(fa[x]){
				sz[fa[x]]+=sz[x];
				if(sz[hvson[fa[x]]]<sz[x])	hvson[fa[x]]=x;
			}
			continue;
		}
		fa[e[x][i]]=x;
		dep[e[x][i]]=dep[x]+1;
		sta[++ta]=e[x][i];
		sz[e[x][i]]=1;
		++i;
	}
}
bool vis[N];
int in[N],out[N],num;
inline void dfs2()
{
	memset(cur,0,sizeof(cur));
	ta=0;
	sta[++ta]=1;
	in[1]=++num;
	tf[1]=1;
	while(ta>0){
		int x=sta[ta];
		if(!hvson[x]){
			out[x]=num;
			--ta;
			continue;
		}
		if(!vis[x]){
			vis[x]=1;
			sta[++ta]=hvson[x];
			in[hvson[x]]=++num;
			tf[hvson[x]]=tf[x];
			continue;
		}
		int &i=cur[x];
		while(i<e[x].size()&&(e[x][i]==fa[x]||e[x][i]==hvson[x]))	++i;
		if(i>=e[x].size()){
			out[x]=num;
			--ta;
			continue;
		}
		sta[++ta]=e[x][i];
		in[e[x][i]]=++num;
		tf[e[x][i]]=e[x][i];
		++i;
	}
}
char op;
int bit[N];
inline void add(int pos,int x)
{
	if(!pos)	return;
	for(int i=pos;i<=n;i+=i&-i){
		bit[i]^=x;
	}
}
inline int query(int pos)
{
	int tmp=0;
	for(int i=pos;i>0;i-=i&-i){
		tmp^=bit[i];
	}
	return tmp;
}
inline int lca(int x,int y)
{
	while(tf[x]!=tf[y]){
		dep[tf[x]]>dep[tf[y]]?x=fa[tf[x]]:y=fa[tf[y]];
	}
	return dep[x]<dep[y]?x:y;
}
int main()
{
	n=read();
	for(int i=1;i<=n;++i){
		a[i]=read();
	}
	for(int i=1;i<n;++i){
		x=read();y=read();
		e[x].pb(y);
		e[y].pb(x);
	}
	dfs1();
	dfs2();
	for(int i=1;i<=n;++i){
		add(in[i],a[i]);
		add(out[i]+1,a[i]);
	}
	qn=read();
	while(qn--){
		op=getchar();
		while(op!='Q'&&op!='C')	op=getchar();
		if(op=='Q'){
			x=read();y=read();
			puts((query(in[x])^query(in[y])^a[lca(x,y)])?"Yes":"No");
		}
		else{
			x=read();y=read();
			add(in[x],a[x]);
			add(out[x]+1,a[x]);
			a[x]=y;
			add(in[x],a[x]);
			add(out[x]+1,a[x]);
		}
	}
	return 0;
}
```