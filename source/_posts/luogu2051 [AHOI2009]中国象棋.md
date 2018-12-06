---
title: luogu2051 [AHOI2009]中国象棋
tag: dp
---
```
f[i][j][k]表示到第i行，其中j列有1个，k列有2个
转移见代码
不要把n和m打错了！！！
不要把n和m打错了！！！
不要把n和m打错了！！！
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
const int N=108,mod=9999973;
int n,m;
int f[N][N][N],ans;
int main()
{
    n=read();m=read();
    f[0][0][0]=1;
    for(int i=1;i<=n;++i){
        for(int k=0;k<=m;++k){
            for(int j=0;j+k<=m;++j){
                f[i][j][k]=f[i-1][j][k];
                if(j>=1){
                    f[i][j][k]=(1ll*f[i-1][j-1][k]*(m-k-j+1)%mod+f[i][j][k])%mod;
                    if(j>=2){
                        f[i][j][k]=(1ll*(m-k-j+2)*(m-k-j+1)/2*f[i-1][j-2][k]%mod+f[i][j][k])%mod;
                    }
                }
                if(k>=1){
                    f[i][j][k]=(1ll*f[i-1][j+1][k-1]*(j+1)%mod+f[i][j][k])%mod;
                    f[i][j][k]=(1ll*(m-k-j+1)*j*f[i-1][j][k-1]%mod+f[i][j][k])%mod;
                    if(k>=2){
                        f[i][j][k]=(1ll*(j+2)*(j+1)/2*f[i-1][j+2][k-2]%mod+f[i][j][k])%mod;
                    }
                }
            }
        }
    }
    /*for(int i=1;i<=n;++i){
        for(int k=0;k<=m;++k){
            for(int j=0;j+k<=m;++j){
                cerr<<i<<' '<<j<<' '<<k<<' '<<f[i][j][k]<<'\n';
            }
        }
    }*/
    for(int j=0;j<=m;++j){
        for(int k=0;j+k<=m;++k){
            ans+=f[n][j][k];
            if(ans>mod){
                ans-=mod;
            }
        }
    }
    printf("%d",ans);
    return 0;
}
```
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
const int N=108,mod=9999973;
int n,m;
int f[N][N],ans;
int main()
{
	n=read();m=read();
	f[0][0]=1;
	for(int i=1;i<=n;++i){
		for(int k=m;k>=0;--k){
			for(int j=m-k;j>=0;--j){
				f[j][k]=f[j][k];
				if(j>=1){
					f[j][k]=(1ll*f[j-1][k]*(m-k-j+1)%mod+f[j][k])%mod;
					if(j>=2){
						f[j][k]=(1ll*(m-k-j+2)*(m-k-j+1)/2*f[j-2][k]%mod+f[j][k])%mod;
					}
				}
				if(k>=1){
					f[j][k]=(1ll*f[j+1][k-1]*(j+1)%mod+f[j][k])%mod;
					f[j][k]=(1ll*(m-k-j+1)*j*f[j][k-1]%mod+f[j][k])%mod;
					if(k>=2){
						f[j][k]=(1ll*(j+2)*(j+1)/2*f[j+2][k-2]%mod+f[j][k])%mod;
					}
				}
			}
		}
	}
	for(int j=0;j<=m;++j){
		for(int k=0;j+k<=m;++k){
			ans+=f[j][k];
			if(ans>mod){
				ans-=mod;
			}
		}
	}
	printf("%d",ans);
	return 0;
}
```