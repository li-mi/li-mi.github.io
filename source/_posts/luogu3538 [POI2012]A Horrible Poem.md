---
title: luogu3538 [POI2012]A Horrible Poem
tag: hash
---
```
hash
1.自然溢出，ll或ull都可以，不需要模数，快，可能会被卡
2.取模，注意永远不要变成负值，值域控制在[0,p)，因为变成负值相当于一种取模
3.双hash：hash1+hash2，可能会被卡常

判断一个字符串的循环节：
枚举len（len整除|s|)
然后判断前|s|-len和后|s|-len个的hash值是否相同
对每个询问，不断除以长度的质因数并判断
总复杂度是O(n+qlogn)
先线性筛求出每个数的最小质因数，然后可以在O(总因数个数)内求出1到n的因数个数
```
<!--more-->
自然溢出
```
// luogu-judger-enable-o2
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
const int N=500008,base=233;
int n,qn;
char s[N];
int l,r;
int hash[N],mi[N];
bool vis[N];
int pri[N],li_pri[N],num;
vector<int> factor[N];
inline void shai()
{
    for(int i=2;i<=n;++i){
        if(!vis[i]){
            pri[++num]=i;
            li_pri[i]=i;
            factor[i].pb(i);
        }
        else{
            factor[i].pb(li_pri[i]);
            int tmp=i;
            tmp/=li_pri[i];
            for(int j=0;j<factor[tmp].size();++j){
                if(factor[i][factor[i].size()-1]!=factor[tmp][j])	factor[i].pb(factor[tmp][j]);
            }
        }
        for(int j=1;j<=n&&pri[j]*i<=n;++j){
            vis[pri[j]*i]=1;
            li_pri[pri[j]*i]=pri[j];
            if(i%pri[j]==0)	break;
        }
    }
}
inline int get_hash(int l,int r)
{
    return hash[r]-hash[l-1]*mi[r-l+1];
}
int main()
{
    n=read();
    //scanf("%s",s+1);
    gets(s+1);
    mi[0]=1;
    for(int i=1;i<=n;++i){
        hash[i]=hash[i-1]*base+s[i];
        mi[i]=mi[i-1]*base;
    }
    shai();
    qn=read();
    while(qn--){
        l=read();r=read();
        if(l==r){
            puts("1");
            continue;
        }
        int tmp=r-l+1,ans=tmp;
        for(int i=0;i<factor[tmp].size();++i){
            while(ans%factor[tmp][i]==0&&get_hash(l,r-ans/factor[tmp][i])==get_hash(l+ans/factor[tmp][i],r)){
                ans/=factor[tmp][i];
            }
        }
        printf("%d\n",ans);
    }
    return 0;
}
```
单hash
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
const int N=500008,base=233,mod=998244353;
int n,qn;
char s[N];
int l,r;
int hash[N],mi[N];
bool vis[N];
int pri[N],li_pri[N],num;
vector<int> factor[N];
inline void shai()
{
	for(int i=2;i<=n;++i){
		if(!vis[i]){
			pri[++num]=i;
			li_pri[i]=i;
			factor[i].pb(i);
		}
		else{
			factor[i].pb(li_pri[i]);
			int tmp=i;
			tmp/=li_pri[i];
			for(int j=0;j<factor[tmp].size();++j){
				if(factor[i][factor[i].size()-1]!=factor[tmp][j])	factor[i].pb(factor[tmp][j]);
			}
		}
		for(int j=1;j<=n&&pri[j]*i<=n;++j){
			vis[pri[j]*i]=1;
			li_pri[pri[j]*i]=pri[j];
			if(i%pri[j]==0)	break;
		}
	}
}
inline int get_hash(int l,int r)
{
	return (hash[r]-1ll*hash[l-1]*mi[r-l+1]%mod+mod)%mod;
}
int main()
{
	n=read();
	//scanf("%s",s+1);
	gets(s+1);
	mi[0]=1;
	for(int i=1;i<=n;++i){
		hash[i]=(1ll*hash[i-1]*base%mod+s[i])%mod;
		mi[i]=1ll*mi[i-1]*base%mod;
	}
	shai();
	qn=read();
	while(qn--){
		l=read();r=read();
		if(l==r){
			puts("1");
			continue;
		}
		int tmp=r-l+1,ans=tmp;
		for(int i=0;i<factor[tmp].size();++i){
			while(ans%factor[tmp][i]==0&&get_hash(l,r-ans/factor[tmp][i])==get_hash(l+ans/factor[tmp][i],r)){
				ans/=factor[tmp][i];
			}
		}
		printf("%d\n",ans);
	}
	return 0;
}
```
双hash
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
const int N=500008,base=233,mod=998244353;
int n,qn;
char s[N];
int l,r;
int hash1[N],mi1[N],hash2[N],mi2[N];
bool vis[N];
int pri[N],li_pri[N],num;
vector<int> factor[N];
inline void shai()
{
	for(int i=2;i<=n;++i){
		if(!vis[i]){
			pri[++num]=i;
			li_pri[i]=i;
			factor[i].pb(i);
		}
		else{
			factor[i].pb(li_pri[i]);
			int tmp=i;
			tmp/=li_pri[i];
			for(int j=0;j<factor[tmp].size();++j){
				if(factor[i][factor[i].size()-1]!=factor[tmp][j])	factor[i].pb(factor[tmp][j]);
			}
		}
		for(int j=1;j<=n&&pri[j]*i<=n;++j){
			vis[pri[j]*i]=1;
			li_pri[pri[j]*i]=pri[j];
			if(i%pri[j]==0)	break;
		}
	}
}
inline int get_hash1(int l,int r)
{
	return hash1[r]-hash1[l-1]*mi1[r-l+1];
} 
inline int get_hash2(int l,int r)
{
	return (hash2[r]-1ll*hash2[l-1]*mi2[r-l+1]%mod+mod)%mod;
}
int main()
{
	n=read();
	//scanf("%s",s+1);
	gets(s+1);
	mi1[0]=mi2[0]=1;
	for(int i=1;i<=n;++i){
		hash1[i]=hash1[i-1]*base+s[i];
		mi1[i]=mi1[i-1]*base; 
		hash2[i]=(1ll*hash2[i-1]*base%mod+s[i])%mod;
		mi2[i]=1ll*mi2[i-1]*base%mod;
	}
	shai();
	qn=read();
	while(qn--){
		l=read();r=read();
		if(l==r){
			puts("1");
			continue;
		}
		int tmp=r-l+1,ans=tmp;
		for(int i=0;i<factor[tmp].size();++i){
			while(ans%factor[tmp][i]==0&&get_hash1(l,r-ans/factor[tmp][i])==get_hash1(l+ans/factor[tmp][i],r)&&get_hash2(l,r-ans/factor[tmp][i])==get_hash2(l+ans/factor[tmp][i],r)){
				ans/=factor[tmp][i];
			}
		}
		printf("%d\n",ans);
	}
	return 0;
}
```