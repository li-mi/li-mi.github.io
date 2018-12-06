---
title: bzoj2964 Boss单挑战
tag: dp
---
相当繁琐的背包
dp_mp[i][j]表示用i回合还剩j点mp能打出的max伤害
f_mp[i]表示i回合用mp能打出的max伤害
转移比较简单，按题目说的做即可
sp同理
然后求出最小需要mini回合能打死boss
dp_hp[i][j]表示第i轮我执行完还剩j血能空出的max回合数
如果存在i，使得dp_hp[i][j]>=mini则yes
否则如果dp_hp[n+1][j]合法，表明我活过前n轮，tie
再否则no

错了一次是因为边界，f应该从0开始，可以不使用
<!--more-->
```cpp
#include<bits/stdc++.h>
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
const int N=1008,NN=18;
int T;
int n,m,hp,mp,sp,dhp,dmp,dsp,x; 
int a[N];
int n1,n2,b[NN],c[NN],y[NN],z[NN];
int dp_mp[N][N],dp_sp[N][N],f_mp[N],f_sp[N],dp_hp[N][N]; 
inline void init()
{
	n=read();m=read();hp=read();mp=read();sp=read();dhp=read();dmp=read();dsp=read();x=read();
	for(int i=1;i<=n;++i){
		a[i]=read();
	} 
	n1=read();
	for(int i=1;i<=n1;++i){
		b[i]=read();y[i]=read();
	}
	n2=read();
	for(int i=1;i<=n2;++i){
		c[i]=read();z[i]=read();
	}
} 
inline void up(int &x,int y)
{
	if(y>x)	x=y;
}
int mini,t;
inline void solve()
{
	memset(dp_mp,-1,sizeof(dp_mp));
	memset(dp_sp,-1,sizeof(dp_sp));
	memset(f_mp,0,sizeof(f_mp));
	memset(f_sp,0,sizeof(f_sp));
	dp_mp[0][mp]=0;
	for(int i=0;i<n;++i){
		for(int j=0;j<=mp;++j){
			if(~dp_mp[i][j]){
				up(dp_mp[i+1][min(j+dmp,mp)],dp_mp[i][j]);
				for(int k=1;k<=n1;++k){
					if(j>=b[k])	up(dp_mp[i+1][j-b[k]],dp_mp[i][j]+y[k]);
				}
			}
		}
	}
	for(int i=0;i<=n;++i){
		for(int j=0;j<=mp;++j)	up(f_mp[i],dp_mp[i][j]);
	}
	dp_sp[0][sp]=0;
	for(int i=0;i<n;++i){
		for(int j=0;j<=sp;++j){
			if(~dp_sp[i][j]){
				up(dp_sp[i+1][min(j+dsp,sp)],dp_sp[i][j]+x);
				for(int k=1;k<=n2;++k){
					if(j>=c[k])	up(dp_sp[i+1][j-c[k]],dp_sp[i][j]+z[k]);
				}
			}
		}
	}
	for(int i=0;i<=n;++i){
		for(int j=0;j<=sp;++j)	up(f_sp[i],dp_sp[i][j]);
	}
	mini=INT_MAX;
	t=n;
	for(int i=0;i<=n;++i){
		while(t>0&&f_mp[i]+f_sp[t-1]>=m)	--t;
		if(f_mp[i]+f_sp[t]>=m)	mini=min(mini,i+t);
	}
	memset(dp_hp,0,sizeof(dp_hp));
	dp_hp[1][hp]=1;
	for(int i=1;i<=n;++i){
		for(int j=1;j<=hp;++j){
			if(dp_hp[i][j]>=mini){
				printf("Yes %d\n",i);
				return;
			}
		}
		for(int j=a[i]+1;j<=hp;++j){
			if(dp_hp[i][j]){
				up(dp_hp[i+1][j-a[i]],dp_hp[i][j]+1);
				up(dp_hp[i+1][min(j-a[i]+dhp,hp)],dp_hp[i][j]);
			}
		}
	}
	for(int i=1;i<=hp;++i){
		if(dp_hp[n+1][i]>0){
			puts("Tie");
			return;
		}
	}
	puts("No");
}
int main()
{
	T=read();
	while(T--){
		init();
		solve();
	}	
	printf("\n");
	return 0;
}
```