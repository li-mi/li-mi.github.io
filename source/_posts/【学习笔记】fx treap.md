---
title: 【学习笔记】fx treap
tag: 平衡树
---
treap有点难写啊
fx treap好！（**没有引用**！）
那么**常数大**也没什么办法。。

fx treap的主体：
```
int rt,tot;
struct node{
	int ls,rs,val,pri,sz;
}fx[N];
```
（假如要写多颗平衡树，把这个东西封装一下就好了）
rt：这颗树的根的编号
tot：总点数（删除的点不会消失，只是它的编号不再被使用了）
ls：左孩子
rs：右孩子
val：关键码（平衡树存的值）
pri：随机的优先级比较值
sz：点数（相同的值并没有合并到一起）

加入一个新的点：
```
int seed=233;
inline int ra()//可以不重复的生成1~INT_MAX-1的每一个数
{
	seed=48271ll*seed%INT_MAX;
	return seed;
}
inline int new_node(int x)
{
	++tot;
	fx[tot].sz=1;
	fx[tot].val=x;
	fx[tot].pri=ra();
	fx[tot].ls=fx[tot].rs=0;
	return tot;
}
```

更新节点的信息：（更改了树的结构的时候需要更新，fx treap需要在merge和split的时候更新）
```
inline void pushup(int x)
{
	fx[x].sz=fx[fx[x].ls].sz+fx[fx[x].rs].sz+1;
	/* more */
}
```

合并(merge)两颗fx，要求左边的fx的每个点的关键码小于右边的fx的每个点的关键码
```
inline int merge(int x,int y)
{
	if(!x||!y)	return x+y;
	if(fx[x].pri<fx[y].pri){
		fx[x].rs=merge(fx[x].rs,y);
		pushup(x);
		return x;
	}
	else{
		fx[y].ls=merge(x,fx[y].ls);
		pushup(y);
		return y;
	}
}
```

分裂(split)一颗fx，可以按值分裂，也可以按排名分裂，这里将键值<=x的分成左树，其余分成右树
split操作会得到两个值，rt_l表示左树的根的编号，rt_r表示右树的根的编号（这样写没有返回值，注意这两个值的变化，要即时的把值存下来，每次split之后他们的值就变了）
```
int rt_l,rt_r;
inline void split(int rt,int x)
{
	if(!rt){
		rt_l=rt_r=0;
		return;
	}
	if(fx[rt].val>x){
		split(fx[rt].ls,x);
		fx[rt].ls=rt_r;
		rt_r=rt;
	}
	else{
		split(fx[rt].rs,x);
		fx[rt].rs=rt_l;
		rt_l=rt;
	}
	pushup(rt);
}
```

拥有了merge和split的fx treap，仍然拥有treap的一切性质，仍然可以使用treap的一切函数
fx treap独特之处在于，它抛弃了旋转操作，那么treap的insert和delete就不适用于fx treap了（只有这两个操作是需要zag和zig的）
插入：
```
inline void inser(int x)
{
	split(rt,x);
	rt=merge(merge(rt_l,new_node(x)),rt_r);
}
```
删除：
```
inline void delet(int x)
{
	split(rt,x);
	int tmp=rt_r;
	split(rt_l,x-1);
	rt=merge(merge(rt_l,merge(fx[rt_r].ls,fx[rt_r].rs)),tmp);
}
```
短小精悍，那么其他操作也可以考虑用fx特有的merge和split来完成（**常数大**）
求键值为x的最小排名（相同的不算）：
```
inline int rank(int x)//rank在c++11中是关键字
{
	split(rt,x-1);
	int ans=fx[rt_l].sz+1;
	rt=merge(rt_l,rt_r);
	return ans;	
}
```
求排名第k的键值：//并不能优化这个过程。。
```
inline int kth(int rt,int k)
{
	if(k>fx[rt].sz)	return -1;
	while(1){
		if(k==fx[fx[rt].ls].sz+1)	return rt;
		if(k<=fx[fx[rt].ls].sz){
			rt=fx[rt].ls;
		}
		else{
			k-=fx[fx[rt].ls].sz+1;
			rt=fx[rt].rs;
		}
	}
}
```
求前驱后继：
```
inline int get_pre(int x)
{
	int ans;
	split(rt,x-1);
	if(!rt_l){
		ans=-INT_MAX;
	}
	else{
		ans=fx[kth(rt_l,fx[rt_l].sz)].val;
	}
	rt=merge(rt_l,rt_r);
	return ans;
}
inline int get_nxt(int x)
{
	int ans;
	split(rt,x);
	if(!rt_r){
		ans=INT_MAX;
	}
	else{
		ans=fx[kth(rt_r,1)].val;
	}
	rt=merge(rt_l,rt_r);
	return ans;
}
```

清空：
```
rt=tot=0;
```