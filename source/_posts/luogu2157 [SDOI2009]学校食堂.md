---
title: luogu2157 [SDOI2009]学校食堂
tag:
- 状压
- dp
---
```
好题
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
const int N=1008;
int T;
int n;
int t[N],b[N];
int f[N][1<<8][18];
int ans;
inline bool ok(int x,int y,int t){
	for(int i=x;i<y;++i){
		if(((t>>(i-x))&1)==0&&i+b[i]<y)	return 0;//注意判不包含的 
	}
	return 1;
}
int main()
{
	T=read();
	while(T--){
		n=read();
		for(int i=1;i<=n;++i){
			t[i]=read();b[i]=read();
		}
		memset(f,0x3f,sizeof(f));
		f[1][0][7]=0;
		for(int i=1;i<=n;++i){
			for(int j=0;j<256;++j){/////// 
			//for(int j=0;j<(1<<(b[i]+1));++j){
				for(int k=0;k<=15;++k){
					if(f[i][j][k]<0x3f3f3f3f){
						if((j&1)){
							f[i+1][j>>1][k-1]=min(f[i+1][j>>1][k-1],f[i][j][k]);
						}
						else{
							int tmp=0x3f3f3f3f;
							for(int p=i;p<=n;++p){
								if((j&(1<<(p-i)))==0){
									//if(!ok(i,p,j))	break;
									
									if(p>tmp)	break;
									tmp=min(tmp,p+b[p]);
									if(i==1&&j==0)	f[i][j|(1<<(p-i))][p-i+8]=0;	
									else	f[i][j|(1<<(p-i))][p-i+8]=min(f[i][j|(1<<(p-i))][p-i+8],f[i][j][k]+(t[i+k-8]^t[p]));
								}
							}
						}
					}
				}
			}
		}
		ans=0x3f3f3f3f;
		for(int k=0;k<=7;++k){
			ans=min(ans,f[n+1][0][k]);
		}
		printf("%d\n",ans);
	}
	return 0;
}
```