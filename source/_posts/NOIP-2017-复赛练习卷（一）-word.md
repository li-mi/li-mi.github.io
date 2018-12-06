---
title: NOIP 2017 复赛练习卷（一)word
date: 2017-06-04 18:24:58
tags:
---
# 题目：
定义：一个 P 单词是指，这个单词不包含 3 个连续的辅音字母，且不包含 3 个连续的元音字母，且至少包含一个字母 L。
现有一个由大写字母与下划线构成的单词，要求将所有下划线替换成大写字母，构成一个新的单词，求构成 P 单词的方案数。
元音字母是指 A，E，I，O，U，其余字母都是辅音字。
<!--more-->
【输入】
一行，一个字符串（长度不超过 100，下划线个数不超过 10）。
【输出】
一行，一个整数，表示构成 P 单词的方案数。
【样例输入一】
L_V
【样例输出一】
5
【样例输入二】
V_ _K
【样例输出二】
10
【样例输入三】
JA_BU_K_A
【样例输出三】
485
# 题解：
将元音字母用0表示，辅音字母用1表示。
dp[i][j]，其中i=0/1，表示第j位选择i时的总方案数。
但是写起来有点麻烦，最后决定听取大佬的建议，换用暴力做↓
暴力枚举10个下滑线，再判断。
第一遍做的时候没看见必须要有一个L，于是gg。。
所有判断原先是否已有L,有了正常做，没有的话↓
将得到的方案数减去没有一个L的方案数。
记住开long long。
# 代码：
```
#include<bits/stdc++.h>
using namespace std;
typedef long long LL;
inline int read()
string s;
int a[108],p[18];
int n,num;
bool suc;
LL ans,tmp=1,gg;
inline bool  check(){
	for(int i=1;i<=n-2;i++) if(a[i]==a[i+1]&&a[i]==a[i+2])	return 0;
	return 1;
}
void dfs(int t)
{
	if(t>num){
		if(check()){
			ans+=tmp;
			if(!suc){
				gg=tmp;
				while(gg%21==0&&gg>0){
					gg=gg/21*20;
				}
				ans-=gg;				
			}
		}
		return;
	}
	for(int i=0;i<=1;i++){
		a[p[t]]=i;
		if(i==0)	tmp*=5;
		else	tmp*=21;
		dfs(t+1);
		if(i==0)	tmp/=5;
		else	tmp/=21;
	}
}
int main()
{
    cin>>s;
    for(int i=0;i<s.size();i++){
		if(s[i]=='L')	suc=1;
		if(s[i]=='_'){
			p[++num]=i+1;
		}
		else if(s[i]=='A'||s[i]=='E'||s[i]=='I'||s[i]=='O'||s[i]=='U')	a[i+1]=0;
		else a[i+1]=1;
    }
    n=s.size();
    dfs(1);
    cout<<ans;
    return 0;
}
```

欢迎转载或引用，能附上lz的网址就更好啦
