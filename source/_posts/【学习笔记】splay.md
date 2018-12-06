---
title: 【学习笔记】splay
tag: splay
---
splay是个好东西
复杂度O(nlogn)（势能分析我不会）
常数较大请写迭代
每次询问后都要做splay操作（一直询问深度为O(n)的点就挂了，例如依次插入1到n）
~~一个常数不太大的模板~~
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
const int N=100008;
int n;
int opt,x;
int tot,rt,ch[N][2],fa[N],key[N],num[N],sum[N];
inline void newnode(int &pos,int p,int x)
{
	pos=++tot;
	fa[pos]=p;
	key[pos]=x;
	num[pos]=sum[pos]=1;
	ch[pos][0]=ch[pos][1]=0;
}
inline void pushup(int x)
{
	sum[x]=sum[ch[x][0]]+sum[ch[x][1]]+num[x];
}
inline void rotate(int x,int op)
{
	int y=fa[x];
	ch[y][!op]=ch[x][op];
	fa[ch[x][op]]=y;
	if(fa[y]){
		ch[fa[y]][ch[fa[y]][1]==y]=x;
	}
	fa[x]=fa[y];
	ch[x][op]=y;
	fa[y]=x;
	
	pushup(y);
	pushup(x); 
}
inline void splay(int x,int goal)
{
	while(fa[x]!=goal){
		if(fa[fa[x]]==goal){
			rotate(x,ch[fa[x]][0]==x);
		}
		else{
			int y=fa[x];
			bool op=ch[fa[y]][0]==y;
			if(ch[y][op]==x){
				rotate(x,!op);
				rotate(x,op);
			}
			else{
				rotate(y,op);
				rotate(x,op);
			}
		}
	}
	if(goal==0){
		rt=x;
	}
}
inline void insert(int x)
{
	if(rt==0){
		newnode(rt,0,x);
		return;
	}
	int tmp=rt;
	while(1){
		if(key[tmp]==x){
			++num[tmp];
			splay(tmp,0);
			return;
		}
		if(ch[tmp][key[tmp]<x]==0)	break;
		tmp=ch[tmp][key[tmp]<x];
	}
	newnode(ch[tmp][key[tmp]<x],tmp,x);
	splay(tot,0);
	return;
}
inline bool find(int x)
{
	int tmp=rt;
	while(tmp){
		if(key[tmp]==x){
			splay(tmp,0);
			return 1;
		}	
		if(key[tmp]>x){
			tmp=ch[tmp][0];
		}
		else{
			tmp=ch[tmp][1];
		}
	}
	return 0;
}
inline void pop()
{
	if(num[rt]>1){
		--num[rt];--sum[rt];return;
	}
	if(!ch[rt][0]){
		fa[ch[rt][1]]=0;
		rt=ch[rt][1];
		return;
	}
	if(!ch[rt][1]){
		fa[ch[rt][0]]=0;
		rt=ch[rt][0];
		return;
	}
	int tmp=ch[rt][0];
	while(ch[tmp][1])	tmp=ch[tmp][1];
	splay(tmp,rt);
	ch[tmp][1]=ch[rt][1];
	rt=tmp;
	fa[tmp]=0;
	fa[ch[rt][1]]=rt;
	
	pushup(rt);
}
inline void del(int x)
{
	if(find(x)){
		pop(); 
	}	
}
inline int get_pre(int x)
{
	int tmp=rt,ans=INT_MIN;
	while(1){
		if(key[tmp]<x){
			ans=max(ans,key[tmp]);
			if(!ch[tmp][1]){
				splay(tmp,0);
				break; 
			} 
			tmp=ch[tmp][1];
		}
		else{
			if(!ch[tmp][0]){
				splay(tmp,0);
				break;
			}
			tmp=ch[tmp][0];
		}
	}
	return ans;
}
inline int get_nxt(int x)
{
	int tmp=rt,ans=INT_MAX;
	while(1){
		if(key[tmp]>x){
			ans=min(ans,key[tmp]);
			if(!ch[tmp][0]){
				splay(tmp,0);
				break;
			}
			tmp=ch[tmp][0];
		}
		else{
			if(!ch[tmp][1]){
				splay(tmp,0);
				break;
			}
			tmp=ch[tmp][1];
		}
	}
	return ans;
}
inline int rank(int x)
{
	int tmp=rt,ans=0;
	while(1){
		if(key[tmp]>=x){
			if(!ch[tmp][0]){
				splay(tmp,0);
				break;
			}
			tmp=ch[tmp][0];
		}
		else{
			ans+=sum[ch[tmp][0]]+num[tmp];
			if(!ch[tmp][1]){
				splay(tmp,0);
				break;
			}
			tmp=ch[tmp][1];
		}
	}
	return ans+1;
}
inline int kth(int x)
{
	int tmp=rt;
	while(x){
		if(sum[ch[tmp][0]]>=x){
			tmp=ch[tmp][0];
		}
		else{
			x-=sum[ch[tmp][0]];
			if(x<=num[tmp]){
				splay(tmp,0);
				return key[tmp];
			}
			x-=num[tmp];
			tmp=ch[tmp][1];
		}
	}
}
int main()
{
	n=read();
	for(int i=1;i<=n;++i){
		opt=read();x=read();
		switch (opt){
			case 1:{
				insert(x);
				break;
			}
			case 2:{
				del(x);
				break;
			}
			case 3:{
				printf("%d\n",rank(x));
				break;
			}
			case 4:{
				printf("%d\n",kth(x));
				break;
			}
			case 5:{
				printf("%d\n",get_pre(x));
				break;
			}
			case 6:{
				printf("%d\n",get_nxt(x));
				break;
			}
		}
	}
	return 0;
}
```

splay最特色的区间翻转
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
const int N=100008;
int n,m;
int l,r,rt,tot;
int fa[N],sz[N],ch[N][2];
bool rev[N];
inline void pushup(int x)
{
	sz[x]=sz[ch[x][0]]+sz[ch[x][1]]+1;
}
inline void rotate(int x,int op)
{
	int y=fa[x];
	ch[y][!op]=ch[x][op];
	fa[ch[x][op]]=y;
	if(fa[y]){
		ch[fa[y]][ch[fa[y]][1]==y]=x;
	}
	fa[x]=fa[y];
	ch[x][op]=y;
	fa[y]=x;
	
	pushup(y);
	pushup(x);
}
inline void splay(int x,int goal)
{
	while(fa[x]!=goal){
		if(fa[fa[x]]==goal){
			rotate(x,ch[fa[x]][0]==x);
		}
		else{
			int y=fa[x];
			bool op=ch[fa[y]][0]==y;
			if(ch[y][op]==x){
				rotate(x,!op);
				rotate(x,op);
			}
			else{
				rotate(y,op);
				rotate(x,op);
			}
		}
	}
	if(goal==0){
		rt=x;
	}
}
inline void pushdown(int x)
{
	if(rev[x]){
		swap(ch[x][0],ch[x][1]);
		rev[ch[x][0]]^=1;
		rev[ch[x][1]]^=1;
		rev[x]=0;
	}
}
inline int find(int x)
{
	int tmp=rt,cnt=0;
	while(1){
		pushdown(tmp);
		if(sz[ch[tmp][0]]+1==x)	return tmp;
		if(sz[ch[tmp][0]]>=x){
			tmp=ch[tmp][0];
		}
		else{
			x-=sz[ch[tmp][0]]+1;
			tmp=ch[tmp][1];
		}
	}
}
inline void rever(int l,int r)
{
	int x=find(l-1),y=find(r+1);
	splay(x,0);
	splay(y,rt);
	rev[ch[y][0]]^=1;
}
inline void build(int p,int l,int r)
{
	if(l>r)	return;
	if(l==r){
		fa[l]=p;
		sz[l]=1;
		if(l<p){
			ch[p][0]=l;
		}	
		else{
			ch[p][1]=l;
		}	
		return;
	}
	int mid=(l+r)>>1;
	fa[mid]=p;
	build(mid,l,mid-1);
	build(mid,mid+1,r);
	pushup(mid);
	if(mid<p){
		ch[p][0]=mid;
	}
	else{
		ch[p][1]=mid;
	}
}
int main()
{
	n=read();m=read();
	build(0,1,n+2);
	rt=(n+3)>>1;
	while(m--){
		l=read()+1;r=read()+1;
		if(l==r)	continue;
		rever(l,r);
	}
	//不能print！！有标记还没下放！！
	for(int i=2;i<=n+1;++i){
		printf("%d ",find(i)-1);
	} 
	return 0;
}
```
