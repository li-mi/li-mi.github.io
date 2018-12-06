---
title: luogu2577 [ZJOI2005]午餐
tag: dp
---
```
套路的贪心
假如只有一个窗口，可以证明按吃饭时间降序排是最优的
先排序
然后将人按顺序分到两个窗口
这样就可以dp了
f[i][j]表示安排完第i个人，第一队的排队时间为j，的最小吃饭时间
sum[i]表示前i人的排队时间和
f[i][j]=min(f[i][j],max(f[i-1][j],sum[i]-j+a[i].y)) (j=0 to sum[i-1])
f[i][j]=min(f[i][j],max(f[i-1][j-a[i].x],j+a[i].x+a[i].y)) (j=a[i].x to sum[i])
观察到上下两个方程的范围正好差一个a[i].x
可以像01背包一样减一维
将j降序遍历

还有一个更厉害的做法
随机化。。
既然是随机化就有风险
随机化数组用到n+1就能a了（不管用不用srand(time(0)))
开到n+10就会出现wa了（用了70，不用80）
随机有风险，考试需谨慎
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
const int N=208;
int n;
struct node{
    int x,y;
}a[N];
inline bool cmp(const node &a,const node &b)
{
    return a.y>b.y;
}
int f[N*N],sum[N];
int ans=INT_MAX;
int main()
{
    n=read();
    for(int i=1;i<=n;++i){
        a[i].x=read();a[i].y=read();
    }
    sort(a+1,a+n+1,cmp);
    for(int i=1;i<=n;++i){
        sum[i]=sum[i-1]+a[i].x;
    }
    
    memset(f,0x3f,sizeof(f));
    f[0]=0;
    for(int i=0;i<n;++i){
        for(int j=sum[i];j>=0;--j){
            if(f[j]==0x3f3f3f3f)	continue;
            f[j+a[i+1].x]=min(f[j+a[i+1].x],max(f[j],j+a[i+1].x+a[i+1].y));
            f[j]=max(f[j],sum[i+1]-j+a[i+1].y);
        }
    }
    for(int j=0;j<=sum[n];++j){
        ans=min(ans,f[j]);
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
const int N=208,lim=1e5;
int n;
int f[N];
struct node{
	int x,y;
}a[N];
int s1,s2,t1,t2,ans=INT_MAX;
inline bool cmp(const node &a,const node &b)
{
	return a.y>b.y;
}
int main()
{
	//srand(time(0));
	n=read();
	for(int i=1;i<=n;++i){
		a[i].x=read();a[i].y=read();
	}
	sort(a+1,a+n+1,cmp);
	for(int i=1;i<=n+1;++i){
		f[i]=i;
	}
	for(int k=1;k<=lim;++k){
		random_shuffle(f+1,f+n+2);
		s1=0;s2=0;t1=0;t2=0;
		for(int i=1;i<=n;++i){
			if(f[i]&1){
				s1+=a[i].x;
				t1=max(t1,s1+a[i].y);
			}
			else{
				s2+=a[i].x;
				t2=max(t2,s2+a[i].y);
			}
		}
		ans=min(ans,max(t1,t2));
	}
	printf("%d",ans);
	return 0;
}
```