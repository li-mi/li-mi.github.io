---
title: hdu1599 find the mincost route
tag: 图论
---
```
无向图找最小环
floyd O(n^3)
枚举三点的环，但要确保有一点不在其他两个点的最短路上，所以一边floyd一边枚举

注意0x3f3f3f3f*3会爆int
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
const int N=108;
int n,m;
int x,y,z;
int w[N][N],dis[N][N],ans;
int main()
{
	while(scanf("%d%d",&n,&m)!=EOF){
		memset(dis,0x3f,sizeof(dis));
		memset(w,0x3f,sizeof(w));
		ans=0x3f3f3f3f;
		for(int i=1;i<=m;++i){
			x=read();y=read();z=read();
			if(w[x][y]>z)	w[x][y]=w[y][x]=dis[x][y]=dis[y][x]=z;
		}
		for(int k=1;k<=n;++k){
			for(int i=1;i<k;++i){
				for(int j=i+1;j<k;++j){
					ans=min((LL)ans,0ll+dis[i][j]+w[i][k]+w[j][k]);
				}
			}
			for(int i=1;i<=n;++i){
				if(dis[i][k]<0x3f3f3f3f){
					for(int j=1;j<=n;++j){
						if(dis[i][k]+dis[k][j]<dis[i][j]){
							dis[i][j]=dis[i][k]+dis[k][j];
						}
					}
				}
			}
		}
		if(ans==0x3f3f3f3f)	puts("It's impossible.");
		else	printf("%d\n",ans);
	}
	return 0;
}
```