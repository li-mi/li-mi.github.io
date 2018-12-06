---
title: luogu3320 [SDOI2015]寻宝游戏
tag: 虚树
---
```
每加入一个点，其实就是在虚树上从前驱和后继向这个点连边
虚树不需要建，用set求前驱后继
细节！！
```
<!--more-->
1.有点麻烦，用dfn求lca要求fa,topfa,dep,dep2都以dfn为下标
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
const int N=100008;
int n,m;
int x,y,z;
int nume,head[N];
struct edge{
	int to,nxt,f;
}e[N<<1];
inline void addedge(int x,int y,int z)
{
	e[++nume]=(edge){y,head[x],z};head[x]=nume;
}
bool has[N];
int num;
int dfn[N],dfc,sz[N],fa[N],dep[N],heavyson[N],topfa[N];
LL dep2[N];
inline void dfs(int x,int f,int p)
{
	dfn[x]=++dfc;
	dep[dfn[x]]=dep[p]+1;
	dep2[dfn[x]]=dep2[p]+f;
	fa[dfn[x]]=p;
	sz[x]=1;
	for(int i=head[x];i;i=e[i].nxt){
		if(!dfn[e[i].to]){
			dfs(e[i].to,e[i].f,dfn[x]);
			sz[x]+=sz[e[i].to];
			if(sz[e[i].to]>sz[heavyson[x]]){
				heavyson[x]=e[i].to;
			}
		}
	}
}
inline void dfs2(int x,int anc)
{
	topfa[dfn[x]]=dfn[anc];
	if(heavyson[x])	dfs2(heavyson[x],anc);
	for(int i=head[x];i;i=e[i].nxt){
		if(dfn[e[i].to]!=fa[dfn[x]]&&e[i].to!=heavyson[x])	dfs2(e[i].to,e[i].to);
	}
}
inline int lca(int x,int y)
{
	while(topfa[x]!=topfa[y]){
		dep[topfa[x]]>dep[topfa[y]]?x=fa[topfa[x]]:y=fa[topfa[y]];
	}
	return dep[x]<dep[y]?x:y;
}
set<int> s;
set<int>::iterator it;
LL ans;
int t1,t2,t3;
inline LL dis(int x,int y)
{
	return dep2[x]+dep2[y]-2*dep2[lca(x,y)];
}
inline void solve()
{
	if(has[x]){
		s.insert(dfn[x]);
		it=s.lower_bound(dfn[x]);
		t1=*it;
		if(it==s.begin()){
			t2=*(--s.end());
		}
		else{
			--it;
			t2=*it;
			++it;
		}
		if(it==--s.end()){
			t3=*s.begin();
		}
		else{
			++it;
			t3=*it;
			--it;
		}
		ans+=dis(t1,t2)+dis(t1,t3)-dis(t2,t3);
	}
	else{
		it=s.lower_bound(dfn[x]);
		t1=*it;
		if(it==s.begin()){
			t2=*(--s.end());
		}
		else{
			--it;
			t2=*it;
			++it;
		}
		if(it==--s.end()){
			t3=*s.begin();
		}
		else{
			++it;
			t3=*it;
			--it;
		}
		ans-=dis(t1,t2)+dis(t1,t3)-dis(t2,t3);
		s.erase(dfn[x]);
	}
	printf("%lld\n",ans);
}
int main()
{
	n=read();m=read();
	for(int i=1;i<n;++i){
		x=read();y=read();z=read();
		addedge(x,y,z);
		addedge(y,x,z);
	}
	
	dfs(1,0,0);
	dfs2(1,1);
	while(m--){
		x=read();
		if(has[x]){
			has[x]=0;
			--num;
		}
		else{
			has[x]=1;
			++num;
		}
		if(num<=1){
			if(has[x]){
				s.insert(dfn[x]);
			}
			else{
				s.erase(dfn[x]);
			}
			puts("0");
		}
		else{
			solve();
		}
	}
	return 0;
}
```
2.记录每个dfn对应的节点，这样不需要改树剖了，但多开一个数组，会更慢。。
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
const int N=100008;
int n,m;
int x,y,z;
int nume,head[N];
struct edge{
	int to,nxt,f;
}e[N<<1];
inline void addedge(int x,int y,int z)
{
	e[++nume]=(edge){y,head[x],z};head[x]=nume;
}
bool has[N];
int num;
int dfn[N],dfc,sz[N],fa[N],dep[N],heavyson[N],topfa[N],pos[N];
LL dep2[N];
inline void dfs(int x)
{
	dfn[x]=++dfc;
	pos[dfc]=x;
	
	sz[x]=1;
	for(int i=head[x];i;i=e[i].nxt){
		if(!dfn[e[i].to]){
			fa[e[i].to]=x;
			dep[e[i].to]=dep[x]+1;
			dep2[e[i].to]=dep2[x]+e[i].f;
			dfs(e[i].to);
			sz[x]+=sz[e[i].to];
			if(sz[e[i].to]>sz[heavyson[x]]){
				heavyson[x]=e[i].to;
			}
		}
	}
}
inline void dfs2(int x,int anc)
{
	topfa[x]=anc;
	if(heavyson[x])	dfs2(heavyson[x],anc);
	for(int i=head[x];i;i=e[i].nxt){
		if(e[i].to!=fa[x]&&e[i].to!=heavyson[x])	dfs2(e[i].to,e[i].to);
	}
}
inline int lca(int x,int y)
{
	while(topfa[x]!=topfa[y]){
		dep[topfa[x]]>dep[topfa[y]]?x=fa[topfa[x]]:y=fa[topfa[y]];
	}
	return dep[x]<dep[y]?x:y;
}
set<int> s;
set<int>::iterator it;
LL ans;
int t1,t2,t3;
inline LL dis(int x,int y)
{
	x=pos[x];y=pos[y];
	return dep2[x]+dep2[y]-2*dep2[lca(x,y)];
}
inline void solve()
{
	if(has[x]){
		s.insert(dfn[x]);
		it=s.lower_bound(dfn[x]);
		t1=*it;
		if(it==s.begin()){
			t2=*(--s.end());
		}
		else{
			--it;
			t2=*it;
			++it;
		}
		if(it==--s.end()){
			t3=*s.begin();
		}
		else{
			++it;
			t3=*it;
			--it;
		}
		ans+=dis(t1,t2)+dis(t1,t3)-dis(t2,t3);
	}
	else{
		it=s.lower_bound(dfn[x]);
		t1=*it;
		if(it==s.begin()){
			t2=*(--s.end());
		}
		else{
			--it;
			t2=*it;
			++it;
		}
		if(it==--s.end()){
			t3=*s.begin();
		}
		else{
			++it;
			t3=*it;
			--it;
		}
		ans-=dis(t1,t2)+dis(t1,t3)-dis(t2,t3);
		s.erase(dfn[x]);
	}
	printf("%lld\n",ans);
}
int main()
{
	n=read();m=read();
	for(int i=1;i<n;++i){
		x=read();y=read();z=read();
		addedge(x,y,z);
		addedge(y,x,z);
	}
	
	dfs(1);
	dfs2(1,1);
	while(m--){
		x=read();
		if(has[x]){
			has[x]=0;
			--num;
		}
		else{
			has[x]=1;
			++num;
		}
		if(num<=1){
			if(has[x]){
				s.insert(dfn[x]);
			}
			else{
				s.erase(dfn[x]);
			}
			puts("0");
		}
		else{
			solve();
		}
	}
	return 0;
}
```
3.set居然可以用struct作cmp，学习了，当然速度超慢
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
const int N=100008;
int n,m;
int x,y,z;
int nume,head[N];
struct edge{
	int to,nxt,f;
}e[N<<1];
inline void addedge(int x,int y,int z)
{
	e[++nume]=(edge){y,head[x],z};head[x]=nume;
}
bool has[N];
int num;
int dfn[N],dfc,sz[N],fa[N],dep[N],heavyson[N],topfa[N];
LL dep2[N];
inline void dfs(int x)
{
	dfn[x]=++dfc;
	sz[x]=1;
	for(int i=head[x];i;i=e[i].nxt){
		if(!dfn[e[i].to]){
			fa[e[i].to]=x;
			dep[e[i].to]=dep[x]+1;
			dep2[e[i].to]=dep2[x]+e[i].f;
			dfs(e[i].to);
			sz[x]+=sz[e[i].to];
			if(sz[e[i].to]>sz[heavyson[x]]){
				heavyson[x]=e[i].to;
			}
		}
	}
}
inline void dfs2(int x,int anc)
{
	topfa[x]=anc;
	if(heavyson[x])	dfs2(heavyson[x],anc);
	for(int i=head[x];i;i=e[i].nxt){
		if(e[i].to!=fa[x]&&e[i].to!=heavyson[x])	dfs2(e[i].to,e[i].to);
	}
}
inline int lca(int x,int y)
{
	while(topfa[x]!=topfa[y]){
		dep[topfa[x]]>dep[topfa[y]]?x=fa[topfa[x]]:y=fa[topfa[y]];
	}
	return dep[x]<dep[y]?x:y;
}
struct cmp{
	bool operator()(const int &x,const int &y){
		return dfn[x]<dfn[y];
	}
};
set<int,cmp> s;
set<int,cmp>::iterator it;
LL ans;
int t1,t2,t3;
inline LL dis(int x,int y)
{
	return dep2[x]+dep2[y]-2*dep2[lca(x,y)];
}
inline void solve()
{
	if(has[x]){
		s.insert(x);
		it=s.lower_bound(x);
		t1=*it;
		if(it==s.begin()){
			t2=*(--s.end());
		}
		else{
			--it;
			t2=*it;
			++it;
		}
		if(it==--s.end()){
			t3=*s.begin();
		}
		else{
			++it;
			t3=*it;
			--it;
		}
		ans+=dis(t1,t2)+dis(t1,t3)-dis(t2,t3);
	}
	else{
		it=s.lower_bound(x);
		t1=*it;
		if(it==s.begin()){
			t2=*(--s.end());
		}
		else{
			--it;
			t2=*it;
			++it;
		}
		if(it==--s.end()){
			t3=*s.begin();
		}
		else{
			++it;
			t3=*it;
			--it;
		}
		ans-=dis(t1,t2)+dis(t1,t3)-dis(t2,t3);
		s.erase(x);
	}
	printf("%lld\n",ans);
}
int main()
{
	n=read();m=read();
	for(int i=1;i<n;++i){
		x=read();y=read();z=read();
		addedge(x,y,z);
		addedge(y,x,z);
	}
	
	dfs(1);
	dfs2(1,1);
	while(m--){
		x=read();
		if(has[x]){
			has[x]=0;
			--num;
		}
		else{
			has[x]=1;
			++num;
		}
		if(num<=1){
			if(has[x]){
				s.insert(x);
			}
			else{
				s.erase(x);
			}
			puts("0");
		}
		else{
			solve();
		}
	}
	return 0;
}
```