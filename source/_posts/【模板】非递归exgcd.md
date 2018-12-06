---
title: 【模板】非递归exgcd
tag:
 - 模板
---
```
a*1+b*0=a
a*0+b*1=b
等式右边作辗转相除
设
ax''+by''=t1
ax' +by' =t2
ax  +by  =t1-t1/t2*t2
令q=t1/t2,r=t1%t2
则
x=x''-qx'
y=y''-qy'
当r=0时结束
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
int a,b,x,y;
inline void exgcd(int a,int b,int &x,int &y)
{
    x=0;y=1;
    int prevx=1,prevy=0;
    int q=a/b,r=a%b,t;
    while(r){
        t=x;
        x=prevx-q*x;
        prevx=t;
        t=y;
        y=prevy-q*y;
        prevy=t;
        
        a=b;
        b=r;
        q=a/b;
        r=a%b;
    }
    //return b;//gcd(a,b)=b
}
int main()
{
    a=read();b=read();
    exgcd(a,b,x,y);
    printf("%d",(x%b+b)%b); 
    return 0;
}
```