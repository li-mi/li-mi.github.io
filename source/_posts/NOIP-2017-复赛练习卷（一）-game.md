---
title: NOIP 2017 复赛练习卷（一)game
date: 2017-06-06 20:56:36
tags:
---
# 题意：
小M在玩一个游戏。游戏有N轮，每一轮，系统给出两个数X和Y，她的任务是将当前得到的所有X和Y两两配对，将每对X、Y求和，使得最大的和最小。
小M算晕了，于是找你帮忙~ 
<!--more-->
【输入格式】
输入第一行包含一个整数N（1<=N<=100000）
接下来N行，每行两个整数X、Y（1<=X,Y<=100） 
【输出格式】
输出共N行，每行一个整数，对于当前得到的所有X和Y进行配对，输出最大和最小的值。 
【样例输入】 
3 
2 8 
3 1 
1 4
【样例输出】
10 
10 
9
【数据范围】
对于50%的数据，N<=200； 
对于100%的数据，N<=100000。
# 题解：
lz也不会。。
但lz有大佬（orz）。
大佬的解法基于↓
![wzc的证明](http://a2.qpic.cn/psb?/V12v3Sbo0PUdJT/5g8Ftm2RHqZFWPXg5BqRO1WEhg*7doeFNTa.oR1INCo!/c/dHUAAAAAAAAA&ek=1&kp=1&pt=0&bo=VQOAAoAHoAURGNk!&tm=1496750400&sce=60-2-2&rf=0-0)
所有只要求{Xi,Yn-i+1}max。
但是lz不会做，于是大佬告诉我，x,y小于100，用类似桶排序的方法做。。
可是lz还是不会，于是大佬在两分钟内给lz打了一份精辟的代码，但lz并没有能力写得如此精辟，于是只能将就的讲一下lz的方法↓
求动态的{Xi,Yn-i+1}max，一般的排序肯定不行。但是x,y的值很小，于是把它们丢到一个[100]的数组里。
每次从x从1到100，y从100到1这样搜，并用tx、ty记录x用了几个，y用了几个。这样的每次求解复杂度只有100*2。
# 代码：
```
#include<bits/stdc++.h>
using namespace std;
typedef long long LL;
inline int read()
{
    int x=0;char ch=getchar();
    while(ch<'0'||ch>'9'){ch=getchar();}
    while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
    return x;
}
int n,t,ans,tx,ty;
int num[2][108];
int main()
{
	n=read();
	while(n--){
		ans=0;tx=0;ty=0;
		for(int i=0;i<=1;i++){
			t=read();
			num[i][t]++;
		}
		int j=101;
		for(int i=1;i<=100;i++){
			if(num[0][i]){
				while(tx>=ty){//只用当tx<ty时，当前i与当前j才会在同一位置
					while(!num[1][--j]);
					ty+=num[1][j];
				}
				ans=max(ans,i+j);
				tx+=num[0][i];
				
			}
		}
		cout<<ans<<'\n';
	}
    return 0;
}
```
欢迎转载或引用，能附上lz的网址就更好啦