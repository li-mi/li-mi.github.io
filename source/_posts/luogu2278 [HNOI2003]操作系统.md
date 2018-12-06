---
title: luogu2278 [HNOI2003]操作系统
tag: 堆
---
```
好久没写重载了
less需要重载小于号
greater需要重载大于号
stl中使用结构体会使成员变量变成private，导致只可访问，不可修改
所以成员函数要加const
以及priority_queue的大小于判断和sort/set是相反的
priority_queue：为1就交换
sort/set：为1不交换
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
int id,t1,t2,a,pre;
struct node{
	int x,y,z;
	inline bool operator < (const node &a) const{
		if(x!=a.x)	return x<a.x;
		return y>a.y;
	}
};
priority_queue<node> q;
int main()
{
	while(scanf("%d",&id)!=EOF){
		t1=read();t2=read();a=read();
		if(q.empty()){
			q.push((node){a,id,t2});
			pre=t1;
		}
		else{
			while(!q.empty()&&pre+q.top().z<=t1){
				pre+=q.top().z;
				printf("%d %d\n",q.top().y,pre);
				q.pop();
			}
			if(!q.empty()&&pre<t1){
				node t=q.top();
				q.pop(); 
				t.z-=t1-pre;
				q.push(t); 
			}
			q.push((node){a,id,t2});
			pre=t1;
		}
	}
	while(!q.empty()){
		pre+=q.top().z;
		printf("%d %d\n",q.top().y,pre);
		q.pop();
	}
	return 0;
}
```