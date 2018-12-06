---
title: luogu1169 [ZJOI2007]棋盘制作
tag: dp
---
```
整张图可以重新分为两类
然后悬线法解决
极大子正方形和极大子矩阵
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
const int N=2008;
int n,m,tmp;
bool a[N][N];
int h[N][N],l[N][N],r[N][N];
int ans1,ans2;
int main()
{
	n=read();m=read();
	for(int i=1;i<=n;++i){
		for(int j=1;j<=m;++j){
			a[i][j]=read();
			if((i+j)%2==0){
				a[i][j]^=1;
			}
		}
	}
	for(int i=1;i<=n;++i){
		for(int j=1;j<=m;++j){
			if(a[i][j]==0){
				h[i][j]=0;
				l[i][j]=0;
			}
			else{
				h[i][j]=h[i-1][j]+1;
				l[i][j]=l[i][j-1]+1;
			}
		}
		for(int j=m;j;--j){
			if(a[i][j]==0){
				r[i][j]=0;
			}
			else{
				r[i][j]=r[i][j+1]+1;
			}
		}
	}
	for(int i=1;i<=n;++i){
		for(int j=1;j<=m;++j){
			if(i>1&&a[i-1][j]==1){
				l[i][j]=min(l[i][j],l[i-1][j]);
				r[i][j]=min(r[i][j],r[i-1][j]);
			}
			tmp=min(h[i][j],l[i][j]+r[i][j]-1);
			ans1=max(ans1,tmp*tmp);
			ans2=max(ans2,h[i][j]*(l[i][j]+r[i][j]-1));
		}
	}
	for(int i=1;i<=n;++i){
		for(int j=1;j<=m;++j){
			if(a[i][j]==1){
				h[i][j]=0;
				l[i][j]=0;
			}
			else{
				h[i][j]=h[i-1][j]+1;
				l[i][j]=l[i][j-1]+1;
			}
		}
		for(int j=m;j;--j){
			if(a[i][j]==1){
				r[i][j]=0;
			}
			else{
				r[i][j]=r[i][j+1]+1;
			}
		}
	}
	for(int i=1;i<=n;++i){
		for(int j=1;j<=m;++j){
			if(i>1&&a[i-1][j]==0){
				l[i][j]=min(l[i][j],l[i-1][j]);
				r[i][j]=min(r[i][j],r[i-1][j]);
			}
			tmp=min(h[i][j],l[i][j]+r[i][j]-1);
			ans1=max(ans1,tmp*tmp);
			ans2=max(ans2,h[i][j]*(l[i][j]+r[i][j]-1));
		}
	}
	printf("%d\n%d",ans1,ans2);
	return 0;
}
```