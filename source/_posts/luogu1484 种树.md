---
title: luogu1484 种树
tag: 
 - 堆
 - 思维
---
```
大根堆
要能够反悔
注意到选了一个，它一定比两边的优
所以要么选它，要么两边同时选
选一个点之后将它两边的值减去它，更新到这个点，删去两边的点
这样每轮还是相当于选一个点
注意边界处理

调了一天
要先弹完用过的再判负
顺序很重要！
顺序很重要！
顺序很重要！
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
const int N=500008;
int n,k;
int pre[N],nxt[N];
LL a[N];
priority_queue<pair<LL,int> > q;
LL ans;
bool vis[N];
int main()
{
	n=read();k=read();
	for(int i=1;i<=n;++i){
		a[i]=read();
		q.push(mp(a[i],i));
		pre[i]=i-1;
		nxt[i]=i+1;
	}
	while(k--){
		while(vis[q.top().second]){//要用while，if的话会消耗k 
			q.pop();
		}
		if(q.top().first<=0)	break;	
		ans+=q.top().first;
		int x=q.top().second;
		q.pop();
		a[x]=a[pre[x]]+a[nxt[x]]-a[x];
		if(pre[x]!=0){
			vis[pre[x]]=1;
			pre[x]=pre[pre[x]];
			nxt[pre[x]]=x;
		}
		if(nxt[x]!=n+1){
			vis[nxt[x]]=1;
			nxt[x]=nxt[nxt[x]];
			pre[nxt[x]]=x;
		}	
		q.push(mp(a[x],x));
	}
	printf("%lld",ans);
	return 0;
}
```