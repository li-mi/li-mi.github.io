---
title: luogu3808 【模板】AC自动机（简单版）
tag: ac自动机
---
```
ac自动机或者trie图
字符集过大不能用trie图，需要map的ac自动机
```
<!--more-->
ac自动机
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
const int N=1000008;
int n;
char s[N];
struct node{
	int ch[26],fail,num;
}t[N];
int sz;
inline void ins()
{
	int u=0,n=strlen(s+1),c;
	for(int i=1;i<=n;++i){
		c=s[i]-'a';
		if(!t[u].ch[c])	t[u].ch[c]=++sz;
		u=t[u].ch[c];
	}
	++t[u].num;
}
int q[N],he,ta;
inline void getAC()
{
	he=1;ta=1;
	int u,v,x;
	for(int i=0;i<26;++i){
		if(t[0].ch[i]){
			q[ta]=t[0].ch[i];
			++ta;
		}
	}
	while(he!=ta){
		u=q[he];
		++he;
		for(int i=0;i<26;++i){
			v=t[u].ch[i];
			if(v){
				x=t[u].fail;
				while(x!=0&&!t[x].ch[i])	x=t[x].fail;
				if(t[x].ch[i])	x=t[x].ch[i];
				t[v].fail=x;
				q[ta]=v;
				++ta;
			}
		}
	}
}
int ans;
bool vis[N];
inline void AC()
{
	int u=0,n=strlen(s+1),c;
	for(int i=1;i<=n;++i){
		c=s[i]-'a';
		while(u!=0&&!t[u].ch[c])	u=t[u].fail;
		if(t[u].ch[c]){
			u=t[u].ch[c];
			for(int v=u;v!=0&&!vis[v];v=t[v].fail){
				ans+=t[v].num;
				vis[v]=1;
			}
		} 
	}
}
int main()
{
	n=read();
	for(int i=1;i<=n;++i){
		scanf("%s",s+1);
		ins();
	}
	getAC();
	scanf("%s",s+1);
	AC();
	printf("%d",ans);
	return 0;
}
```
trie图
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
const int N=1000008;
int n;
char s[N];
struct node{
	int ch[26],fail,num;
}t[N];
int sz;
inline void ins()
{
	int u=0,n=strlen(s+1),c;
	for(int i=1;i<=n;++i){
		c=s[i]-'a';
		if(!t[u].ch[c])	t[u].ch[c]=++sz;
		u=t[u].ch[c];
	}
	++t[u].num;
}
int q[N],he,ta;
inline void getAC()
{
	he=1;ta=1;
	int u,v,x;
	for(int i=0;i<26;++i){
		if(t[0].ch[i]){
			q[ta]=t[0].ch[i];
			++ta;
		}
	}
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
		}
	}
}
int ans;
bool vis[N];
inline void AC()
{
	int u=0,n=strlen(s+1),c;
	for(int i=1;i<=n;++i){
		c=s[i]-'a';
		u=t[u].ch[c];
		for(int v=u;v!=0&&!vis[v];v=t[v].fail){
			ans+=t[v].num;
			vis[v]=1;
		}
	}
}
int main()
{
	n=read();
	for(int i=1;i<=n;++i){
		scanf("%s",s+1);
		ins();
	}
	getAC();
	scanf("%s",s+1);
	AC();
	printf("%d",ans);
	return 0;
}
```