---
title: luogu2336 [SCOI2012]喵星球上的点名
tag: 
- ac自动机
- fail树
- 树状数组
---
```
也是神题了
字符串综合题
各种做法

ac自动机暴力过不了？
假的
zyz的暴力跑得飞快
而且很难卡（我想了半天也构造不出来）

一个有复杂度保证的做法
Q1就是fail树中一个点的子树中有多少个不同的标记
对于同一个名字
将它在fail树中所有的点按dfn排序，在dfs序上打标记
每个点自身+1，和前一个点的lca处-1，这样可以保证每个名字在一颗子树中只产生1的贡献
Q2就是问一个点到根有多少点名串
在in的地方+1，out+1的地方-1，这样可以保证每个名字只会考虑在它的根的路径上的点
还是要减掉lca
```
<!--more-->
有复杂度保证的做法
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
int len,x;
vector<int> a[N];
map<int,int> t[N];
int sz,fail[N];
int nume,head[N];
struct edge{
	int to,nxt;
}e[N];
inline void addedge(int x,int y)
{
	e[++nume]=(edge){y,head[x]};head[x]=nume;
}
int pos[N];
inline void inser(int id)
{
	int u=0;
	len=read();
	for(int i=1;i<=len;++i){
		x=read();
		if(!t[u][x]){
			t[u][x]=++sz;
		}
		u=t[u][x];
	}
	pos[id]=u;
}
int q[N],he,ta;
map<int,int>::iterator it;
inline void getfail()
{
	he=1;ta=2;
	q[1]=0;
	while(he!=ta){
		int x=q[he];
		++he;
		for(it=t[x].begin();it!=t[x].end();++it){
			int t1=it->first,t2=it->second;
			if(x==0){
				fail[t2]=0;
				addedge(0,t2);
			}
			else{
				int k=fail[x];
				while(k!=0&&t[k].find(t1)==t[k].end()){
					k=fail[k];
				}
				int tmp=t[k][t1];
				fail[t2]=tmp;
				addedge(tmp,t2);
			}
			q[ta]=t2;
			++ta;
		}
	}
}
int in[N],out[N],dfc,fa[N][20],dep[N];
inline void dfs(int x)
{
	in[x]=++dfc;
	for(int i=1;i<=18;++i){
		if(dep[x]>=(1<<i)){
			fa[x][i]=fa[fa[x][i-1]][i-1];
		}
	}
	for(int i=head[x];i;i=e[i].nxt){
		if(e[i].to!=fa[x][0]){
			fa[e[i].to][0]=x;
			dep[e[i].to]=dep[x]+1;
			dfs(e[i].to);
		}
	}
	out[x]=dfc;
}
inline int lca(int x,int y)
{
	if(x==y)	return x;
	if(dep[x]<dep[y])	swap(x,y);
	for(int i=18;i>=0;--i){
		if(dep[x]>=dep[y]+(1<<i)){
			x=fa[x][i];
		}
	}
	if(x==y)	return x;
	for(int i=18;i>=0;--i){
		if(fa[x][i]!=fa[y][i]){
			x=fa[x][i];
			y=fa[y][i];
		}
	}
	return fa[x][0];
}

int bit1[N],bit2[N];
inline void add(int *bit,int x,int pos)
{
	for(int i=pos;i<=dfc;i+=i&-i){
		bit[i]+=x;
	}
}
inline int query(int *bit,int pos)
{
	int tmp=0;
	for(int i=pos;i;i-=i&-i){
		tmp+=bit[i];
	}
	return tmp;
} 

int b[N],vis[N],ans[N];
inline bool cmp(const int &a,const int &b)
{
	return in[a]<in[b];
}
inline void solve(int id)
{
	int u=0,len=0;
	for(int i=0;i<a[id].size();++i){
		while(u&&t[u].find(a[id][i])==t[u].end()){
			u=fail[u];
		}
		if(t[u].find(a[id][i])!=t[u].end()){
			u=t[u][a[id][i]];
		}
		if(vis[u]!=id)	b[++len]=u;
		vis[u]=id;
	}
	sort(b+1,b+len+1,cmp);
	ans[id]+=query(bit1,in[b[1]]);
	add(bit2,1,in[b[1]]);
	for(int i=2;i<=len;++i){
		ans[id]+=query(bit1,in[b[i]]);
		ans[id]-=query(bit1,in[lca(b[i],b[i-1])]);
		add(bit2,1,in[b[i]]);
		add(bit2,-1,in[lca(b[i],b[i-1])]);
	}
}
int main()
{
	n=read();m=read();
	for(int i=1;i<=n;++i){
		len=read();
		while(len--){
			x=read();
			a[i].pb(x);
		}
		a[i].pb(-1);
		len=read();
		while(len--){
			x=read();
			a[i].pb(x);
		}
	}
	for(int i=1;i<=m;++i){
		inser(i);
	}
	getfail();
	dfs(0);
	for(int i=1;i<=m;++i){
		add(bit1,1,in[pos[i]]);
		add(bit1,-1,out[pos[i]]+1);
	}
	for(int i=1;i<=n;++i){
		solve(i);
	} 
	for(int i=1;i<=m;++i){
		printf("%d\n",query(bit2,out[pos[i]])-query(bit2,in[pos[i]]-1));
	}
	for(int i=1;i<=n;++i){
		printf("%d ",ans[i]);
	}
	return 0;
}
```
暴力。。
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
int ans1[N],ans2[N];
vector<int> a[N],V;
int num[N],pos[N],q[N];
map<int,int> to[N];
bool vis[N];
int sz;
struct data{
    int fail[N];
    inline void insert(int id){
        int L=read(),x;
        int u=0;
        for(int i=1;i<=L;i++)
        {
            x=read();
            if(!to[u][x])to[u][x]=++sz;
            u=to[u][x];
        }
        ++num[u];
        pos[id]=u;
    }
    inline void buildfail(){
        int head=0,tail=0;
       	for(map<int,int>::iterator i=to[0].begin();i!=to[0].end();++i){
			q[tail++]=i->second;
       	}
        while(head!=tail)
        {
            int x=q[head];head++;
            for(map<int,int>::iterator i=to[x].begin();i!=to[x].end();++i)
            {
                int t=i->first,k=fail[x];
                while(k&&to[k].find(t)==to[k].end()){
					k=fail[k];
                }	
                if(to[k].find(t)!=to[k].end())	fail[i->second]=to[k][t];
                q[tail++]=i->second;
            }
        }
    }
    inline void get(int id,int x){
        for(int i=x;i;i=fail[i])
            if(!vis[i])
            {
                vis[i]=1;V.push_back(i);
                ++ans1[i];
                ans2[id]+=num[i];
            }
            else return;
    }
    inline void solve(int x){
        int u=0;
        for(int i=0;i<a[x].size();i++)
        {
            int t=a[x][i];
            while(u&&to[u].find(t)==to[u].end())	u=fail[u];
            if(to[u].find(t)!=to[u].end())	u=to[u][t];
			get(x,u);
        }
        for(int i=0;i<V.size();i++)vis[V[i]]=0;
        V.clear();
    }
}trie;
int main()
{
    n=read();m=read();
    int L,x;
    for(int i=1;i<=n;i++)
    {
        L=read();
        while(L--)x=read(),a[i].push_back(x);
        a[i].push_back(-1);
        L=read();
        while(L--)x=read(),a[i].push_back(x);
    }
    for(int i=1;i<=m;i++)
        trie.insert(i);
    trie.buildfail();
    for(int i=1;i<=n;i++)
        trie.solve(i);
    for(int i=1;i<=m;i++)printf("%d\n",ans1[pos[i]]);
    for(int i=1;i<=n;i++)
    {
        printf("%d",ans2[i]);
        if(i!=n)printf(" ");
    }
    return 0;
}
```