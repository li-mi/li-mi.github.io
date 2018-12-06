---
title: luogu3233 [HNOI2014]世界树
tag: 
- 虚树
- 倍增
---
```
显然是虚树
然而怎么dp。。

看题解看题解。。
先两遍dfs求出虚树上每个点到哪个点最近
注意到虚树上的一条链所属的点一定和某一个端点是一样的（为什么我没有注意到）
然后就是看两个端点是否是属于同一个点
是的话说明整条链就是这个点的
不是的话可以算出中间的分割点的深度

倍增可以求节点a的深度为x的祖先
倍增大概有3种写法，若用dep[fa[x][i]]>=dep[y]，请将根节点的dep设为1
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
const int N=300008;
int num,n,m,qn;
int x,y;
int nume,head[N];
struct edge{
    int to,nxt;
}e[N<<1];
inline void addedge(int x,int y)
{
    e[++nume]=(edge){y,head[x]};head[x]=nume;
}
int dfn[N],dfc,sz[N],fa[N][20],dep[N];
inline void dfs(int x)
{
    dfn[x]=++dfc;
    sz[x]=1;
    for(int i=1;(1<<i)<=dep[x];++i){
        fa[x][i]=fa[fa[x][i-1]][i-1];
    }
    for(int i=head[x];i;i=e[i].nxt){
        if(e[i].to!=fa[x][0]){
            fa[e[i].to][0]=x;
            dep[e[i].to]=dep[x]+1;
            dfs(e[i].to);
            sz[x]+=sz[e[i].to];
        }
    }
}
inline int lca(int x,int y)
{
    if(x==y)	return x;
    if(dep[x]<dep[y])	swap(x,y);
    for(int i=19;i>=0;--i){
        if(dep[x]>=dep[y]+(1<<i)){//dep[fa[x][i]]>=dep[y] (1<<i)&bin
            x=fa[x][i];
        }
    }
    if(x==y)	return x;
    for(int i=19;i>=0;--i){
        if(fa[x][i]!=fa[y][i]){
            x=fa[x][i];
            y=fa[y][i];
        }
    }
    return fa[x][0];	
}
int a[N],t[N],ori[N];
int s[N],ta;
int par[N],dis[N];
PII g[N];
int rem[N];
int ans[N];
inline bool cmp(const int &a,const int &b)
{
    return dfn[a]<dfn[b];
}
inline void addedge_2(int x,int y)
{
    par[y]=x;
    dis[y]=dep[y]-dep[x];
}
inline void dp()
{
    for(int i=n;i>1;--i){
        int t1=t[i],t2=par[t1];
        g[t2]=min(g[t2],mp(g[t1].first+dis[t1],g[t1].second));
    }
    for(int i=2;i<=n;++i){
        int t1=t[i],t2=par[t1];
        g[t1]=min(g[t1],mp(g[t2].first+dis[t1],g[t2].second));
    }
}
inline int find(int x,int pos)
{
    for(int i=19;i>=0;--i){
        if(dep[x]>=pos+(1<<i)){
            x=fa[x][i];
        }
    }
    return x;
}
inline void solve()
{
    m=read();n=0;
    for(int i=1;i<=m;++i){
        t[++n]=ori[i]=a[i]=read();
        g[a[i]]=mp(0,a[i]);
    }
    sort(a+1,a+m+1,cmp);
    
    ta=0;
    for(int i=1;i<=m;++i){
        if(!ta){
            s[++ta]=a[i];
        }
        else{
            int tmp=lca(a[i],s[ta]);
            while(dfn[tmp]<dfn[s[ta]]){
                if(dfn[tmp]>=dfn[s[ta-1]]){
                    addedge_2(tmp,s[ta]);
                    --ta;
                    if(s[ta]!=tmp){
                        s[++ta]=tmp;
                        t[++n]=tmp;
                        g[tmp]=mp(0x3f3f3f3f,0);
                    }
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
    while(ta>1){
        addedge_2(s[ta-1],s[ta]);
        --ta;
    }
    sort(t+1,t+n+1,cmp);
    dp();
    for(int i=1;i<=n;++i){
        rem[t[i]]=sz[t[i]];
    }
    ans[g[t[1]].second]=num-sz[t[1]];
    
    for(int i=2;i<=n;++i){
        int t1=t[i],t2=par[t1];//par[t1]=0;
        int tmp=find(t1,dep[t2]-1);
        rem[t2]-=sz[tmp];
        if(g[t1].second==g[t2].second){
            ans[g[t1].second]+=sz[tmp]-sz[t1];
        }
        else{
            int len=g[t1].first+g[t2].first+dis[t1],mid=dep[t1]-(len/2-g[t1].first);
            if(!(len&1)&&g[t1].second>g[t2].second)	++mid;
            int tmp2=find(t1,mid);
            ans[g[t1].second]+=sz[tmp2]-sz[t1];
            ans[g[t2].second]+=sz[tmp]-sz[tmp2];
        }
    }
    
    for(int i=1;i<=n;++i){
        ans[g[t[i]].second]+=rem[t[i]];
    }
    for(int i=1;i<=m;++i){
        printf("%d ",ans[ori[i]]);
        ans[ori[i]]=0;
    }
    puts("");
}
int main()
{
    num=n=read();
    for(int i=1;i<n;++i){
        x=read();y=read();
        addedge(x,y);
        addedge(y,x);
    }
    dfs(1);
    qn=read();
    while(qn--){
        solve();
    }
    return 0;
}
```