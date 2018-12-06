---
title: noi模拟赛 狮鹫旅行
tag:
- 图论
- 矩阵
---

# Griffin
##  Description
狮鹫是联盟的主要交通工具，在艾泽拉斯的土地上散布着n个狮鹫站点，有m条有向的飞行线路，也就是构成了一张n个点m条边的有向图。这m条线路被分成了c种等级，对于其中的第i种等级，只有声望达到了wi才能乘坐狮鹫飞过这条线路。小C现在在暴风城（1号点），他要赶到暗月马戏团（n号点）参加他学长的电音表演。作为一个喜欢藏锋的健美先生，初始时他声望为0。但他实在是太健美了，以至于他每乘坐一次狮鹫（即经过一条边）就能使他的声望 +1 。为了更好地藏锋，小C不会通过其他方法来提升声望。 小C不喜欢步行，也不会开传送门，所以他不会通过除乘坐狮鹫以外的其他方法来到达另一个站点。现在他想知道是否能到达，如果能，他还想知道他最少要经过多少条边才能到达。
<!--more-->
## Input
从文件griffin.in中读入数据.
第一行三个整数n, m, c.
接下来m行，每行三个整数u, v, lv，表示有一条u到v的线路，等级为lv.
接下来一行c个整数，第i个表示wi.
## Output
输出到文件griffin.out中.
若不能到达输出Impossible。
否则输出一个整数表示他最少要经过的边数。
## Sample1
- Input
3 2 2
1 2 1
2 3 2
0 2
- Output
Impossible
## Sample2
- Input
3 3 2
1 2 1
2 1 1
2 3 2
0 2
- Output
4

```
%dy
对矩阵和图的关系的理解又加深了
这张图一开始所有边都是上锁的
当你每达到一定等级就能解锁
转移矩阵会逐渐扩大

过程大概是：
先走val[i]-val[i-1]步转移到当前可用的点(state)（一个一维向量）
若state为空，说明无解
否则就可以解锁了，扩展一下转移矩阵(omat)
解锁当前等级后判断一下能不能到达n，用bfs

用bitset优化
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
const int N=188,M=40008,C=58;
int n,m,cn;
int val[C];
struct data{
	int u,v,w;
	inline bool operator <(const data &b) const{
		return w<b.w;
	}
}arr[M];
bitset<N> state[N],omat[N],mat[N],tmp[N];//state就是一个1维向量 
int cur,ans;
int q[N],dis[N];
inline void update(bitset<N> *a,bitset<N> *b)
{
	for(int i=1;i<=n;++i){
		tmp[i].reset();
	}
	for(int i=1;i<=n;++i){
		for(int k=1;k<=n;++k){
			if(a[i][k]){
				tmp[i]|=b[k];
			}
		}
	}
	for(int i=1;i<=n;++i){
		a[i]=tmp[i];
	}
} 
inline int bfs()
{
	int he=1,ta=1;
	memset(dis,0x3f,sizeof(dis));
	for(int i=1;i<=n;++i){
		if(state[1][i]){
			q[ta]=i;
			++ta;
			dis[i]=0;
		}
	}
	while(he!=ta){
		int x=q[he];
		++he;
		for(int i=1;i<=n;++i){
			if(omat[x][i]&&dis[i]>dis[x]+1){
				dis[i]=dis[x]+1;
				q[ta]=i;
				++ta;
			} 
		} 
	}
	return dis[n];
}
int main()
{
	n=read();m=read();cn=read();
	for(int i=1;i<=m;++i){
		arr[i].u=read();
		arr[i].v=read();
		arr[i].w=read();
	}
	sort(arr+1,arr+m+1);
	for(int i=1;i<=cn;++i){
		val[i]=read();
	}
	state[1][1]=1;
	cur=1;
	ans=0x3f3f3f3f;
	for(int i=1;i<=cn;++i){
		int x=val[i]-val[i-1];
		for(int j=1;j<=n;++j){
			mat[j]=omat[j];
		} 
		while(x){
			if(x&1)	update(state,mat);
			update(mat,mat);
			x>>=1; 
		} 
		for(int j=1;j<=n;++j){
			if(state[j].any())	break;
			if(j==n){
				puts("Impossible");
				return 0;
			}
		}
		while(cur<=m&&arr[cur].w<=i){
			if(!omat[arr[cur].u][arr[cur].v]){
				omat[arr[cur].u][arr[cur].v]=1;
			}
			++cur;
		}
		int tmp=bfs();
		ans=min(ans,tmp+val[i]);
	}
	if(ans<0x3f3f3f3f)	printf("%d",ans);
	else	puts("Impossible");
	return 0;
}
```