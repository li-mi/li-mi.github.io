---
title: '[Usaco2015 Open Gold]Palindromic Paths'
date: 2017-06-02 23:26:21
tags:
---
# 题意：
从 n×n 的矩阵 左上角走到右下角会有一个长度 n+n+1 的字符串，问有多少种走法使得路径字符串为回文？

<!--more-->
题目链接：https://www.luogu.org/problem/show?pid=3126 （支持洛谷

# 题解：
哇哇哇本人蒟蒻做了好久其实不难。。
n=500。。好像只用O(n^3)的方法可以过。。
那就dp吧，从（1,1）和（n,n）同时向中间走（lz的第一反应是双向bfs，超时，改成从两头的dp）
状态大概就是dp（i,j,k,l），表示走到（i,j）和（k,l）时的方案数，转移↓
dp[i][j][k][l]=dp[i-1][j][k+1][l]+dp[i-1][j][k][l+1]+dp[i][j-1][k+1][l]+dp[i][j-1][k][l+1]
但是好像会超时超空间。。状态得修改一下↓
时间优化：既然是从两头同时走，可以记一下走的步数，再留一个i和k就可以了，这样就变成O(n^3)。
dp（s,i,k）像这样就可以了。。
lz在网上看到了另一种状态设计↓
dp[i][j][k]表示左上结束节点是第i条副对角线上的第j个点，右下结束节点是第n*2-i条副对角线上的第k个点，构成回文的方案数。（引用地址：http://www.cnblogs.com/czllgzmzl/p/5645030.html）
其实，我也是按照这位大佬的方法做的，因为k表示起来简单啊（但也有麻烦的地方）
转移为dp[s][i][k]=dp[s-1][i-1][k-1]+dp[s-1][i-1][k]+dp[s-1][i][k-1]+dp[s-1][i][k]
完了吗？
有没有发现要开500^3的数组，爆空间了。。
空间优化：dp最常见的空间优化-->滚动数组（能状压的大佬请无视蒟蒻）
第一维好像只需要2个（每次只跟前一次状态有关），这样就可以压了

# 代码：
```
#include<bits/stdc++.h>
using namespace std;
typedef long long LL;
const int MOD=1000000007;
int n;
LL dp[2][508][508],ans;
char a[508][508];
bool now,pre;
int main()
{
    cin>>n;
	for(int i=1;i<=n;i++)	cin>>a[i]+1; 
	if(a[1][1]!=a[n][n]){
		cout<<'0';
		return 0;
	}
	now=1;pre=0;
	dp[now][1][1]=1;
	for(int step=3;step<=n+1;step++){//从第二条副对角线到第n+1条，就是走的步数
		swap(now,pre);
		for(int i=1;i<step;i++){
			for(int k=1;k<step;k++){
				if(a[i][step-i]==a[n-step+1+k][n-k+1])//细心地推一下右下角点的坐标，容易错
					dp[now][i][k]=(dp[pre][i-1][k-1]+dp[pre][i-1][k]+dp[pre][i][k-1]+dp[pre][i][k])%MOD;
				else	dp[now][i][k]=0;//滚动数组一定要清零，不然后面会误用前面的值
			}
		}
	}
	for(int i=1;i<=n;i++)	ans=(ans+dp[now][i][i])%MOD;
	cout<<ans;
    return 0;
}
```

欢迎转载或引用，能附上lz的网址就更好啦