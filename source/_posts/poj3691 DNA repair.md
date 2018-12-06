---
title: poj3691 DNA repair
tag:
- ac自动机
- dp
---
```
在ac自动机上dp
f[i][j]表示匹配串到第i位，在ac自动机上跑到j位置的最小修改次数
ac自动机上每个点要或上fail树上的每一个祖先
可以在求fail指针之后求出
多测注意情况数组和变量（变量变量变量）
sz没清空然后re好久
```
<!--more-->
```
#include<cstdio>
#include<iostream>
#include<cstring>
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
const int N=1008;
int n,cas,mapping[128];
char s[N];
struct node{
	int ch[5],fail;
	bool num;
}t[N];
int sz;
inline void inser()
{
	int u=0,c,n=strlen(s+1);
	for(int i=1;i<=n;++i){
		c=mapping[s[i]];
		if(!t[u].ch[c])	t[u].ch[c]=++sz;
		u=t[u].ch[c];
	}
	t[u].num=1;
}
int q[N];
inline void get_ac()
{
	int he=1,ta=1;
	for(int i=1;i<=4;++i){
		if(t[0].ch[i]){
			q[ta]=t[0].ch[i];
			++ta;
		}
	}
	while(he!=ta){
		int x=q[he];
		++he;
		t[x].num|=t[t[x].fail].num;
		for(int i=1;i<=4;++i){
			if(!t[x].ch[i]){
				t[x].ch[i]=t[t[x].fail].ch[i];
			}
			else{
				t[t[x].ch[i]].fail=t[t[x].fail].ch[i];
				q[ta]=t[x].ch[i];
				++ta;
			}
		}
	}
}
int f[N][N],ans;
inline void solve()
{
	memset(f,0x3f,sizeof(f));
	int n=strlen(s+1);
	f[0][0]=0;
	for(int i=0;i<n;++i){
		for(int j=0;j<=sz;++j){
			if(f[i][j]<0x3f3f3f3f){
				int tmp=t[j].ch[mapping[s[i+1]]];
				if(!t[tmp].num){
					f[i+1][tmp]=min(f[i+1][tmp],f[i][j]);
				}
				for(int k=1;k<=4;++k){
					if(!t[t[j].ch[k]].num)
						f[i+1][t[j].ch[k]]=min(f[i+1][t[j].ch[k]],f[i][j]+1);
				}
			}
		}
	}
	ans=0x3f3f3f3f;
	for(int i=0;i<=sz;++i){
		ans=min(ans,f[n][i]);
	}
	printf("Case %d: ",++cas);
	if(ans==0x3f3f3f3f)	puts("-1");
	else	printf("%d\n",ans);
}
int main()
{
	mapping['A']=1;mapping['G']=2;mapping['C']=3;mapping['T']=4;
	while((n=read())){
		memset(t,0,sizeof(t));
		sz=0;////
		for(int i=1;i<=n;++i){
			scanf("%s",s+1);
			inser();
		}
		scanf("%s",s+1);
		get_ac();
		solve();
	}
	return 0;
}
```