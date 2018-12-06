---
title: luoguP2414 [NOI2011]阿狸的打字机
tag: 
- ac自动机
- 树状数组
---
先做完bzoj3172 [Tjoi2013]单词再看这题
觉得思路清爽多了

答案是x在fail树上的子树和
但是只能保留第y个串的字符
按照字符顺序处理正好能求出当前串的字符
所以想到离线处理询问
按y排序
将fail树转成dfs序
在dfs序上做单点修改，区间求和

第二次做
fail树是一个很重要的东西
fa是自己的最长后缀
每个点是一个前缀
在fail树上某个点的孩子/孙子/曾孙
表明自己在它孩子/孙子/曾孙中作为这个前缀的后缀出现
错了一次是因为dfn和sz搞错
trie的sz是不包括0的
fail树的dfn是包括0的
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
const int N=100008;
char s[N];
int n;
struct node{
	int ch[26],fail,fa;
}t[N];
int sz,num,pos[N];
int m;
struct node_q{
	int x,y,id;
}que[N];
inline bool cmp(const node_q &a,const node_q &b)
{
	return a.y<b.y;
}
int ans[N];
inline void build()
{
	int u=0,c;
	for(int i=1;i<=n;++i){
		if(s[i]=='P'){
			pos[++num]=u;
		}
		else if(s[i]=='B'){
			u=t[u].fa;
		}
		else{
			c=s[i]-'a';
			if(!t[u].ch[c])	t[u].ch[c]=++sz;
			t[t[u].ch[c]].fa=u;
			u=t[u].ch[c];
		}
	}
}
int q[N],he,ta;
int nume,head[N];
struct edge{
	int to,nxt;
}e[N];
inline void addedge(int x,int y)
{
	e[++nume]=(edge){y,head[x]};head[x]=nume;
}
inline void getAC()
{
	he=1;ta=1;
	for(int i=0;i<26;++i){
		if(t[0].ch[i]){
			q[ta]=t[0].ch[i];
			++ta;
			addedge(0,t[0].ch[i]);
		}
	}
	int u,v;
	while(he!=ta){
		u=q[he];
		++he;
		for(int i=0;i<26;++i){
			v=t[u].ch[i];
			if(!v){
				t[u].ch[i]=t[t[u].fail].ch[i];
				continue;
			}
			t[v].fail=t[t[u].fail].ch[i];
			q[ta]=v;
			++ta;
			addedge(t[v].fail,v);
		}
	}
}
int din[N],dout[N],dfn;
inline void dfs(int x)
{
	din[x]=++dfn;
	for(int i=head[x];i;i=e[i].nxt){
		dfs(e[i].to);
	}
	dout[x]=dfn;
}
int bit[N];
inline void add(int pos,int x)
{
	for(int i=pos;i<=dfn;i+=i&-i){
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
inline void solve()
{
	int u=0,qn=0,p=1,c,l,r;
	for(int i=1;i<=n;++i){
		if(s[i]=='P'){
			++qn;
			while(qn==que[p].y){
				l=din[pos[que[p].x]];
				r=dout[pos[que[p].x]];
				ans[que[p].id]=sum(r)-sum(l-1);				
				++p;
			}
		}
		else if(s[i]=='B'){
			add(din[u],-1);
			u=t[u].fa;
		}
		else{
			c=s[i]-'a';
			u=t[u].ch[c];
			add(din[u],1);
		}
	}
}
int main()
{
	scanf("%s",s+1);
	n=strlen(s+1);
	build();
	getAC();
	dfs(0);
	m=read();
	for(int i=1;i<=m;++i){
		que[i].x=read();que[i].y=read();que[i].id=i;
	}
	sort(que+1,que+m+1,cmp);
	solve();
	for(int i=1;i<=m;++i){
		printf("%d\n",ans[i]);
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
const int N=100008;
int n;
char s[N];
struct trie{
	int ch[26],f,fa;
}ac[N];
int sz;
int pos[N],num;
inline void build()
{
	int u=0,c;
	for(int i=1;i<=n;++i){
		if(s[i]=='B'){
			u=ac[u].fa;
		}
		else if(s[i]=='P'){
			pos[++num]=u;
		}
		else{
			c=s[i]-'a';
			if(!ac[u].ch[c])	ac[u].ch[c]=++sz;
			ac[ac[u].ch[c]].fa=u;
			u=ac[u].ch[c];
		}
	} 
}
int q[N],he,ta;
vector<int> e[N];
inline void get_acam()
{
	he=ta=1;
	for(int i=0;i<26;++i){
		if(ac[0].ch[i]){
			q[ta]=ac[0].ch[i];
			++ta;
			e[0].pb(ac[0].ch[i]);
		}
	}
	while(he!=ta){
		int u=q[he];++he;
		for(int i=0;i<26;++i){
			int v=ac[u].ch[i];
			if(v){
				ac[v].f=ac[ac[u].f].ch[i];
				q[ta]=v;
				++ta;
				e[ac[v].f].pb(v);
			}
			else{
				ac[u].ch[i]=ac[ac[u].f].ch[i];
			}
		}
	}
}
int in[N],out[N],dfn;
inline void dfs(int x)
{
	in[x]=++dfn;
	for(int i=0;i<e[x].size();++i){
		dfs(e[x][i]);
	}
	out[x]=dfn;
}
int qn;
struct que{
	int x,y,id;
}qx[N];
inline bool cmp(const que &a,const que &b)
{
	return a.y<b.y;
}
int bit[N];
inline void add(int pos,int x)
{
	if(!pos)	return;
	for(int i=pos;i<=dfn;i+=i&-i){//dfn=sz+1，sz没有算0！！ 
		bit[i]+=x;
	}
}
inline int sum(int pos)
{
	int tmp=0;
	for(int i=pos;i>0;i-=i&-i){
		tmp+=bit[i];
	}
	return tmp;
}
int ans[N];
inline void solve()
{
	int u=0,c,cnt=0,t=1,l,r;
	for(int i=1;i<=n;++i){
		if(s[i]=='B'){
			add(in[u],-1);
			u=ac[u].fa;
		}
		else if(s[i]=='P'){
			++cnt;
			while(cnt==qx[t].y){
				l=in[pos[qx[t].x]];
				r=out[pos[qx[t].x]];
				ans[qx[t].id]=sum(r)-sum(l-1);
				++t;
			}
		}
		else{
			c=s[i]-'a';
			u=ac[u].ch[c];
			add(in[u],1);
		}
	}
}
int main()
{      
	scanf("%s",s+1);
	n=strlen(s+1);
	build();
	get_acam();
	dfs(0);
	qn=read();
	for(int i=1;i<=qn;++i){
		qx[i].x=read();
		qx[i].y=read();
		qx[i].id=i;
	}
	sort(qx+1,qx+qn+1,cmp);
	solve();
	for(int i=1;i<=qn;++i){
		printf("%d\n",ans[i]);
	}
	return 0;
}
```