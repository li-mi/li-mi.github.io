---
title: luogu2763 试题库问题
tag:
 - 网络流
---
```
基础网络流，不要打错变量名！！
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
const int K=28,N=1008;
int n,m,k,p,tmp;
int s,t;
int req[K];
int ans;

int nume=1,head[N+K],cur[N+K];
struct node{
    int to,nxt,f;
}e[N*K*2];
inline void addedge(int x,int y,int z)
{
    e[++nume]=(node){y,head[x],z};head[x]=nume;
    e[++nume]=(node){x,head[y],0};head[y]=nume;
}

int dis[N+K];
int he,ta,q[N+K];
inline bool bfs()
{
    memset(dis,0,sizeof(dis));
    he=1;ta=2;
    q[1]=s;
    dis[s]=1;
    while(he!=ta){
        int x=q[he];
        ++he;
        for(int i=head[x];i;i=e[i].nxt){
            if(e[i].f&&!dis[e[i].to]){
                dis[e[i].to]=dis[x]+1;
                if(e[i].to==t)	return 1;
                q[ta]=e[i].to;
                ++ta;
            }
        }
    }
    return 0;
}
inline int dfs(int x,int low)
{
    if(x==t||!low)	return low;
    int flow=0,tmp;
    for(int &i=cur[x];i;i=e[i].nxt){
        if(dis[e[i].to]==dis[x]+1&&(tmp=dfs(e[i].to,min(low,e[i].f)))){
            flow+=tmp;
            low-=tmp;
            e[i].f-=tmp;
            e[i^1].f+=tmp;
            if(!low)	return flow;
        }
    }
    if(low)	dis[x]=0;
    return flow;
}
inline int dinic()
{
    int maxflow=0;
    while(bfs()){
        for(int i=0;i<=t;++i){
            cur[i]=head[i];
        }
        maxflow+=dfs(s,INT_MAX);
    }
    return maxflow;
}
vector<int> plan[K];
inline void print()
{
    for(int i=1;i<=n;++i){
        for(int j=head[i];j;j=e[j].nxt){
            if(j%2==0&&e[j].f==0){//i/j
                plan[e[j].to-n].pb(i);//i/j
                break;
            }
        }
    }
    for(int i=1;i<=k;++i){
        printf("%d:",i);
        for(int j=0;j<plan[i].size();++j){
            printf(" %d",plan[i][j]);
        }
        puts("");
    }
}
int main()
{
    k=read();n=read();
    s=0;t=n+k+1;
    for(int i=1;i<=k;++i){
        req[i]=read();
        addedge(i+n,t,req[i]);
        m+=req[i];
    }
    for(int i=1;i<=n;++i){
        addedge(s,i,1);
        p=read();
        for(int j=1;j<=p;++j){
            tmp=read();
            addedge(i,tmp+n,1);
        }
    }
    ans=dinic();
    if(ans!=m){
        puts("No Solution!");
    }
    else{
        print();
    }
    return 0;
}
```