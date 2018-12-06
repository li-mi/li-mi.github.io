---
title: luogu2447 [SDOI2010]外星千足虫
tag:
 - 高斯消元
---
```
裸的异或方程组
注意记录最多用到哪个方程
不用bitset不开O2会t3个点
当然要用bitset啦（操作复杂度为bitset除以字长，大概就是cpu一次能处理字长长度的bit位吧——bx2k）
最快的同学是一边读一边高斯消元的，高级
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
const int N=1008,M=2008;
int n,m;
int a[M][N];
char c;
int now=1,ans;
inline bool gauss()
{
	register int i,j,k;
	for(int j=1;j<=n;++j){
		if(a[now][j]==0){
			for(i=now+1;i<=m&&a[i][j]==0;++i);
			if(i>m)	return 0;//有一个自由变量，多解 
			ans=max(ans,i);//当前访问的最大行 
			for(k=j;k<=n+1;++k){
				swap(a[now][k],a[i][k]);
			}
		}
		ans=max(ans,j);
		for(int i=1;i<=m;++i){
			if(now!=i&&a[i][j]){
				for(k=j;k<=n+1;++k){
					a[i][k]^=a[now][k];
				}
			}
		}
		++now;
	}
	return 1;
}
int main()
{
	n=read();m=read();
	for(int i=1;i<=m;++i){
		for(int j=1;j<=n+1;++j){
			c=getchar();
			while(c!='0'&&c!='1')	c=getchar();
			if(c=='1'){
				a[i][j]=1;
			}
		}
	}
	if(gauss()){
		printf("%d\n",ans); 
		for(int i=1;i<=n;++i){
			if(a[i][n+1])	puts("?y7M#");
			else	puts("Earth");
		} 
	}
	else{
		puts("Cannot Determine");
	}
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
const int N=1008,M=2008;
int n,m;
bitset<N> a[M];
char c;
int now=1,ans;
inline bool gauss()
{
	register int i,j,k;
	for(int j=1;j<=n;++j){
		if(a[now][j]==0){
			for(i=now+1;i<=m&&a[i][j]==0;++i);
			if(i>m)	return 0;
			ans=max(ans,i);
			swap(a[now],a[i]);
		}
		ans=max(ans,j);
		for(int i=1;i<=m;++i){
			if(now!=i&&a[i][j]){
				a[i]^=a[now];
			}
		}
		++now;
	}
	return 1;
}
int main()
{
	n=read();m=read();
	for(int i=1;i<=m;++i){
		for(int j=1;j<=n+1;++j){
			c=getchar();
			while(c!='0'&&c!='1')	c=getchar();
			if(c=='1'){
				a[i][j]=1;
			}
		}
	}
	if(gauss()){
		printf("%d\n",ans); 
		for(int i=1;i<=n;++i){
			if(a[i][n+1])	puts("?y7M#");
			else	puts("Earth");
		} 
	}
	else{
		puts("Cannot Determine");
	}
	return 0;
}
```
```
别人的代码↓
```
```
// luogu-judger-enable-o2
#include<cstdio>
#include<algorithm>
#include<cstring>
#include<bitset>
using namespace std;
int gi()
{
    int x=0,w=1;char ch=getchar();
    while ((ch<'0'||ch>'9')&&ch!='-') ch=getchar();
    if (ch=='-') w=0,ch=getchar();
    while (ch>='0'&&ch<='9') x=(x<<3)+(x<<1)+ch-'0',ch=getchar();
    return w?x:-x;
}
const int N = 1005;
int n,m,ele,sol[N];
char s[N];
bitset<N>a[N],tmp;
int main()
{
    n=gi();m=gi();
    for (int ans=1;ans<=m;++ans)
    {
        scanf("%s",s+1);int x=gi();
        for (int i=1;i<=n;++i) tmp[i]=s[i]-'0';tmp[n+1]=x;
        for (int i=1;i<=n;++i)
        {
            if (!tmp[i]) continue;
            if (!a[i][i]) {a[i]=tmp;++ele;break;}
            tmp^=a[i];
        }
        if (ele==n)
        {
            printf("%d\n",ans);
            for (int i=n;i;--i)
            {
                sol[i]=a[i][n+1];
                for (int j=n;j>i;--j)
                    if (a[i][j]) sol[i]^=sol[j];
            }
            for (int i=1;i<=n;++i) puts(sol[i]?"?y7M#":"Earth");
            return 0;
        }
    }
    puts("Cannot Determine");return 0;
}

```