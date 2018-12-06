---
title: 【学习笔记】虚树 luogu2495 [SDOI2011]消耗战
tag: 
- 虚树
- 树上dp
---
```
在树形dp的基础上
保留关键点
每个点记录的信息要是从这个点到根的

构造的方法
1.将关键点排序，相邻的两点求lca，这样所有的点对lca都包含在其中（反证法证明）。在将它们放一起排序，去重，用栈连边。复杂度O(nlogn)（常数大）
2.栈中时刻维护一条链，每加入一个点要维护到这个新的点的链，过程中连边。复杂度O(nlogn)（常数小，难理解）

方法1注意：包括lca的那个数组要开两倍大（或者记录一下哪些数已经出现了），vector要清空
```
<!--more-->
方法1
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
const int N=250008;
int n;
int u,v,w;
int nume,head[N];
struct edge{
    int to,nxt,f;
}e[N<<1];
inline void addedge(int x,int y,int z)
{
    e[++nume]=(edge){y,head[x],z};head[x]=nume;
}
int sz[N],fa[N],dep[N],heavyson[N],topfa[N];
LL mini[N];
int dfn[N],dfc;
inline void dfs(int x)
{
	dfn[x]=++dfc;
    ++sz[x];
    for(int i=head[x];i;i=e[i].nxt){
        if(e[i].to!=fa[x]){
            fa[e[i].to]=x;
            dep[e[i].to]=dep[x]+1;
            mini[e[i].to]=min(mini[x],(LL)e[i].f);
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
inline void build()
{
    mini[1]=LONG_LONG_MAX;
    dfs(1);
    dfs2(1,1);
}
int qn;
int s[N],ta;
bool key[N];
vector<int> vis;
inline bool cmp(const int &a,const int &b)
{
    return dfn[a]<dfn[b];
}
inline void addedge_2(int x,int y)
{
    e[++nume]=(edge){y,head[x]};head[x]=nume;
}
LL f[N];
inline LL dp(int x)
{
    LL tmp=0;
    for(int i=head[x];i;i=e[i].nxt){
        tmp+=dp(e[i].to);
    }
    if(key[x])	f[x]=mini[x];
    else	f[x]=min(mini[x],tmp);
    head[x]=0;
    return f[x];
}
int a[N<<1];//// 
inline void solve()
{
    nume=0;
    n=read();
    for(int i=1;i<=n;++i){
        a[i]=read();
        key[a[i]]=1;
        vis.pb(a[i]);
    }
    sort(a+1,a+n+1,cmp);
    for(int i=n;i>1;--i){
        a[++n]=lca(a[i],a[i-1]);
    }
    sort(a+1,a+n+1,cmp);
    n=unique(a+1,a+n+1)-a-1;
    
    ta=0;
    for(int i=1;i<=n;++i){
        if(ta==0){
            s[++ta]=a[i];
        }
        else{
            while(dfn[s[ta]]+sz[s[ta]]<=dfn[a[i]]){
                --ta;
            }
            addedge_2(s[ta],a[i]);
            s[++ta]=a[i];
        }
    }
    dp(a[1]);
    for(int i=0;i<vis.size();++i){
        key[vis[i]]=0;
    }
    vis.clear();////////////
    printf("%lld\n",f[a[1]]);
}
int main()
{
    n=read();
    for(int i=1;i<n;++i){
        u=read();v=read();w=read();
        addedge(u,v,w);
        addedge(v,u,w);
    }
    build();
    qn=read();
    memset(head,0,sizeof(head));
    while(qn--){
        solve();
    }
    return 0;
}
```
方法2
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
const int N=250008;
int n;
int u,v,w;
int nume,head[N];
struct edge{
    int to,nxt,f;
}e[N<<1];
inline void addedge(int x,int y,int z)
{
    e[++nume]=(edge){y,head[x],z};head[x]=nume;
}
int sz[N],fa[N],dep[N],heavyson[N],topfa[N];
LL mini[N];
int dfn[N],dfc;
inline void dfs(int x)
{
	dfn[x]=++dfc;
    ++sz[x];
    for(int i=head[x];i;i=e[i].nxt){
        if(e[i].to!=fa[x]){
            fa[e[i].to]=x;
            dep[e[i].to]=dep[x]+1;
            mini[e[i].to]=min(mini[x],(LL)e[i].f);
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
inline void build()
{
    mini[1]=LONG_LONG_MAX;
    dfs(1);
    dfs2(1,1);
}
int qn;
int s[N],ta;
bool key[N];
inline bool cmp(const int &a,const int &b)
{
    return dfn[a]<dfn[b];
}
inline void addedge_2(int x,int y)
{
    e[++nume]=(edge){y,head[x]};head[x]=nume;
}
LL f[N];
inline LL dp(int x)
{
    LL tmp=0;
    for(int i=head[x];i;i=e[i].nxt){
        tmp+=dp(e[i].to);
    }
    if(key[x])	f[x]=mini[x];
    else	f[x]=min(mini[x],tmp);
    head[x]=0;
    return f[x];
}
int a[N<<1];//// 
inline void solve()
{
    nume=0;
    n=read();
    for(int i=1;i<=n;++i){
        a[i]=read();
        key[a[i]]=1;
    }
    sort(a+1,a+n+1,cmp);
    
    ta=0;
    for(int i=1;i<=n;++i){
        if(ta==0){
            s[++ta]=a[i];
        }
        else{
            int tmp=lca(s[ta],a[i]);
            while(dfn[tmp]<dfn[s[ta]]){
				if(dfn[tmp]>=dfn[s[ta-1]]){
					addedge_2(tmp,s[ta]);
					--ta;
					if(s[ta]!=tmp)	s[++ta]=tmp;
					break;
				}
				else{
					addedge_2(s[ta-1],s[ta]);
					--ta;
				}
            }
            s[++ta]=a[i];
        }
    }
    while(ta!=1){
		addedge_2(s[ta-1],s[ta]);
		--ta;
    }
    dp(s[1]);
    for(int i=1;i<=n;++i){
		key[a[i]]=0;
    }
    printf("%lld\n",f[s[1]]);
}
int main()
{
    n=read();
    for(int i=1;i<n;++i){
        u=read();v=read();w=read();
        addedge(u,v,w);
        addedge(v,u,w);
    }
    build();
    qn=read();
    memset(head,0,sizeof(head));
    while(qn--){
        solve();
    }
    return 0;
}
```