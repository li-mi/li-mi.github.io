---
title: luogu2216 [HAOI2007]理想的正方形
tag:
- 单调队列
- st
---
```
裸题
```
<!--more-->
st有两种
[讲得很清楚的博客](http://www.xuebuyuan.com/3210463.html)
O(nmlogm) - O(n)
O(nmlognlogm) - O(1)
这题是求正方形，可以O(nmlogt) - O(1)
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
const int N=1008;
int n,m,t;
int a[N][N],maxi[N][N][8],mini[N][N][8];
int len,ans=INT_MAX;
int main()
{
	n=read();m=read();t=read();
	memset(mini,0x3f,sizeof(maxi));
	for(int i=1;i<=n;++i){
		for(int j=1;j<=m;++j){
			maxi[i][j][0]=mini[i][j][0]=a[i][j]=read();
		}
	}
	for(int k=1;(1<<k)<=t;++k){
		for(int i=1;i+(1<<k)-1<=n;++i){
			for(int j=1;j+(1<<k)-1<=m;++j){
				maxi[i][j][k]=max(max(maxi[i][j][k-1],maxi[i+(1<<(k-1))][j][k-1]),
								  max(maxi[i][j+(1<<(k-1))][k-1],maxi[i+(1<<(k-1))][j+(1<<(k-1))][k-1]));
				mini[i][j][k]=min(min(mini[i][j][k-1],mini[i+(1<<(k-1))][j][k-1]),
								  min(mini[i][j+(1<<(k-1))][k-1],mini[i+(1<<(k-1))][j+(1<<(k-1))][k-1])); 
			}
		}
	}
	//len=0;
	//while((1<<(len+1))<t)	++len;
	len=log2(t);
	for(int i=1;i+t-1<=n;++i){
		for(int j=1;j+t-1<=m;++j){
			ans=min(ans,max(max(maxi[i][j][len],maxi[i+t-(1<<len)][j][len]),max(maxi[i][j+t-(1<<len)][len],maxi[i+t-(1<<len)][j+t-(1<<len)][len]))
						-min(min(mini[i][j][len],mini[i+t-(1<<len)][j][len]),min(mini[i][j+t-(1<<len)][len],mini[i+t-(1<<len)][j+t-(1<<len)][len])));
		}
	}
	printf("%d",ans);
	return 0;
}
```
O(nm)的单调队列
写错是因为变量名打错！！
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
const int N=1008;
int n,m,t;
int a[N][N],maxi[N][N],mini[N][N];
int q[N],pos[N],he,ta; 
int main()
{
	n=read();m=read();t=read();
	for(int i=1;i<=n;++i){
		for(int j=1;j<=m;++j){
			a[i][j]=read();
		}
	}
	//----------------
	for(int i=1;i<=n;++i){
		he=1;ta=0;
		for(int j=1;j<t;++j){
			while(he<=ta&&a[i][j]>=q[ta])	--ta;		
			q[++ta]=a[i][j];
			pos[ta]=j;
		}
		for(int j=t;j<=m;++j){
			while(he<=ta&&pos[he]<=j-t)	++he;
			while(he<=ta&&a[i][j]>=q[ta])	--ta;		
			q[++ta]=a[i][j];
			pos[ta]=j;
			maxi[i][j]=q[he];
		}
	}/*
	for(int i=1;i<=n;++i){
		for(int j=1;j<=m;++j){
			cout<<maxi[i][j]<<' ';
		}cout<<'\n';
	}
	puts("");*/
	for(int j=t;j<=m;++j){
		he=1;ta=0;
		for(int i=1;i<t;++i){
			while(he<=ta&&maxi[i][j]>=q[ta])	--ta;
			q[++ta]=maxi[i][j];
			pos[ta]=i;
		}
		for(int i=t;i<=n;++i){
			while(he<=ta&&pos[he]<=i-t)	++he;
			while(he<=ta&&maxi[i][j]>=q[ta])	--ta;
			q[++ta]=maxi[i][j];
			pos[ta]=i;
			maxi[i][j]=q[he];
		}
	}/*
	for(int i=1;i<=n;++i){
		for(int j=1;j<=m;++j){
			cout<<maxi[i][j]<<' ';
		}cout<<'\n';
	}
	puts("");*/
	//--------------------------
	for(int i=1;i<=n;++i){
		he=1;ta=0;
		for(int j=1;j<t;++j){
			while(he<=ta&&a[i][j]<=q[ta])	--ta;		
			q[++ta]=a[i][j];
			pos[ta]=j;
		}
		for(int j=t;j<=m;++j){
			while(he<=ta&&pos[he]<=j-t)	++he;
			while(he<=ta&&a[i][j]<=q[ta])	--ta;		
			q[++ta]=a[i][j];
			pos[ta]=j;
			mini[i][j]=q[he];
		}
	}/*
	for(int i=1;i<=n;++i){
		for(int j=1;j<=m;++j){
			cout<<mini[i][j]<<' ';
		}cout<<'\n';
	}
	puts("");*/
	for(int j=t;j<=m;++j){
		he=1;ta=0;
		for(int i=1;i<t;++i){
			while(he<=ta&&mini[i][j]<=q[ta])	--ta;
			q[++ta]=mini[i][j];
			pos[ta]=i;
		}
		for(int i=t;i<=n;++i){
			while(he<=ta&&pos[he]<=i-t)	++he;
			while(he<=ta&&mini[i][j]<=q[ta])	--ta;
			q[++ta]=mini[i][j];
			pos[ta]=i;
			mini[i][j]=q[he];
		}
	}
	/*for(int i=1;i<=n;++i){
		for(int j=1;j<=m;++j){
			cout<<mini[i][j]<<' ';
		}cout<<'\n';
	}
	puts("");*/
	int ans=INT_MAX;
	for(int i=t;i<=n;++i){
		for(int j=t;j<=m;++j){
			ans=min(ans,maxi[i][j]-mini[i][j]);
		}
	}
	printf("%d",ans);
	return 0;
}
```
