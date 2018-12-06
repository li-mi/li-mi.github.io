---
title: luogu3502 [POI2010]Hamsters
tag:
- hash
- dp
- 矩阵
- 倍增
---
```
因为单词数比较少
不要一个字符一个字符考虑
考虑一个单词到一个单词的转移
求出i到j的转移（用hash，复杂度是O(sum(len)*n))
题目变成求出在n个点的图上走k步最少的边权和
k小的话可以O(nk)dp
k大的话考虑dp优化，矩阵+倍增，预处理出走2的幂步的最小边权和

犯了一堆错：
i/j打错
mi[0]没有初始化
少break，break后不执行++i!
数组开小了
hash和mi要求同类型
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
const int N=208,L=100008,base=233;
int n,m;
char s[L];
int len[N];
vector<LL> hash[N];//LL?
//LL hash[N][L];
LL mi[L];
LL f[35][N][N],ans[N][N],change[N][N],cnt;
inline int get_len(int x,int y)
{
	int tmp;
	if(x==y)	tmp=len[x]-1;
	else	tmp=min(len[x]-1,len[y]-1);
	for(int i=tmp;i>=0;--i){
		if(hash[x][len[x]]-hash[x][len[x]-i]*mi[i]==hash[y][i]){
			return len[y]-i;
		}
	}
}
int main()
{
	n=read();m=read();--m;
	mi[0]=1;////
	for(int i=1;i<L;++i){
		mi[i]=mi[i-1]*base;
	}
	for(int i=1;i<=n;++i){
		scanf("%s",s+1);
		len[i]=strlen(s+1);
		hash[i].resize(len[i]+1,0);
		for(int j=1;j<=len[i];++j){
			hash[i][j]=hash[i][j-1]*base+s[j];
		}
	} 
    memset(f,0x3f,sizeof(f));
    for(int i=1;i<=n;i++){
		for(int j=1;j<=n;j++){
			f[0][i][j]=get_len(i,j);
        }
    }
   
	int tmp;
	for(tmp=1;(1<<tmp)<=m;++tmp){
		for(int i=1;i<=n;++i){
			for(int j=1;j<=n;++j){
				for(int k=1;k<=n;++k){
					f[tmp][i][j]=min(f[tmp][i][j],f[tmp-1][i][k]+f[tmp-1][k][j]);
				}
			}
		}
	}
	for(tmp=0;(1<<tmp)<=m;++tmp){
		if((m>>tmp)&1){
			for(int i=1;i<=n;++i){
				for(int j=1;j<=n;++j){
					ans[i][j]=f[tmp][i][j];
				}
			}
			break;
		}
	} 
	for(++tmp;(1<<tmp)<=m;++tmp){
		if((m>>tmp)&1){
			memset(change,0x3f,sizeof(change));
			for(int i=1;i<=n;++i){
				for(int j=1;j<=n;++j){
					for(int k=1;k<=n;++k){
						change[i][j]=min(change[i][j],min(ans[i][k]+f[tmp][k][j],f[tmp][i][k]+ans[k][j]));
					}
				}
			}
			for(int i=1;i<=n;++i){
				for(int j=1;j<=n;++j){
					ans[i][j]=change[i][j];
				}
			}
		}
	}
	cnt=LONG_LONG_MAX;
	for(int i=1;i<=n;++i){
		for(int j=1;j<=n;++j){
			cnt=min(cnt,len[i]+ans[i][j]);
		}
	}
	printf("%lld",cnt);
	return 0;
}
``