---
title: luogu2331 [SCOI2005]最大子矩阵
tag: dp
---
```
简单的dp
对于已经走过的区域，不管有没有选择，都不会管了
对于当前的i和j，一定是考虑选这个i或j，而不是之前的（因为之前的在之前被考虑）
```
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
const int N=108;
int n,m,k;
int a[N],b[N],sum1[N],sum2[N];
int f[N][11],f2[N][N][11];
int ans;
int main()
{
	n=read();m=read();k=read();
	if(m==1){
		for(int i=1;i<=n;++i){
			a[i]=read();
			sum1[i]=sum1[i-1]+a[i];
		}
		for(int i=0;i<=n;++i){
			for(int j=1;j<=k;++j){
				f[i][j]=-0x3f3f3f3f;//INT_MIN不行，INT_MIN再减东西变成正的了。。 
			}
		}
		for(int i=1;i<=n;++i){
			for(int j=1;j<=k;++j){
				f[i][j]=max(f[i][j],f[i-1][j]);
				for(int p=0;p<i;++p){
					f[i][j]=max(f[i][j],f[p][j-1]+sum1[i]-sum1[p]);
				}
			}
		}
		for(int j=0;j<=k;++j){
			ans=max(ans,f[n][j]);
		}
		printf("%d",ans);
	}
	else{
		for(int i=1;i<=n;++i){
			a[i]=read();b[i]=read();
			sum1[i]=sum1[i-1]+a[i];sum2[i]=sum2[i-1]+b[i];
		}
		for(int i=0;i<=n;++i){
			for(int j=0;j<=n;++j){
				for(int p=1;p<=k;++p){
					f2[i][j][p]=-0x3f3f3f3f;
				}
			}
		}
		for(int i=1;i<=n;++i){
			for(int j=1;j<=n;++j){
				for(int p=1;p<=k;++p){
					f2[i][j][p]=max(f2[i][j][p],f2[i-1][j][p]);
					f2[i][j][p]=max(f2[i][j][p],f2[i][j-1][p]);
					for(int q=0;q<i;++q){
						f2[i][j][p]=max(f2[i][j][p],f2[q][j][p-1]+sum1[i]-sum1[q]);
					}
					for(int q=0;q<j;++q){
						f2[i][j][p]=max(f2[i][j][p],f2[i][q][p-1]+sum2[j]-sum2[q]);
					}
					if(i==j){
						for(int q=0;q<i;++q){
							f2[i][j][p]=max(f2[i][j][p],f2[q][q][p-1]+sum1[i]-sum1[q]+sum2[j]-sum2[q]);
						}
					}
				}
			}
		}
		for(int p=0;p<=k;++p){
			ans=max(ans,f2[n][n][p]);
		}
		printf("%d",ans);
	}
	return 0;
}
/*
10 1 4
1 2 -3 4 -1 6 -8 7 -9 10
*/
```