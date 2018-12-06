---
title: luogu4503 [CTSC2014]企鹅QQ
tag:
- hash
---
```
单hash被卡我也很无奈（明明很正常好吗）
但是rk1居然是单hash。。
233无敌啊
不过我233就a不了
挺简单的就是要双hash
hzw的做法也挺强的
前后各一边hash，每次底数不一样
反正不要让删掉的那一位有贡献就行
怎么做你开心就好
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
const int N=30008,L=208,base=521,base2=1999;
int n,l;
char s[N][L];
int len[N];
LL hash[N],mi[L],hash2[N],mi2[L];
struct node{
    int x,y;
}tmp[N];
int cnt,ans;
inline bool cmp(const node &a,const node &b)
{
    if(a.x!=b.x)	return a.x<b.x;
    return a.y<b.y;
}
int main()
{
    n=read();l=read();cnt=read();
    mi[0]=mi2[0]=1;
    for(int i=1;i<L;++i){
        mi[i]=mi[i-1]*base;
        mi2[i]=mi2[i-1]*base2;
    }
    for(int i=1;i<=n;++i){
        scanf("%s",s[i]+1);
        for(int j=1;j<=l;++j){
            hash[i]=hash[i]*base+s[i][j];
            hash2[i]=hash2[i]*base2+s[i][j]; 
        }
    }
    
    for(int i=1;i<=l;++i){
        for(int j=1;j<=n;++j){
            tmp[j].x=hash[j]-s[j][i]*mi[l-i];
            tmp[j].y=hash2[j]-s[j][i]*mi2[l-i];
        }
        sort(tmp+1,tmp+n+1,cmp);
        cnt=1;
        for(int j=2;j<=n;++j){
            if(tmp[j].x==tmp[j-1].x&&tmp[j].y==tmp[j-1].y){
                ans+=cnt;
                ++cnt;
            }
            else{
                cnt=1;
            }
        }
    }
    printf("%d",ans);
    return 0;
}
```