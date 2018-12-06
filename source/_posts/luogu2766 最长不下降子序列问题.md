---
title: luogu2766 最长不下降子序列问题
tag: 网络流
---
```
第一问lis
第二问和第三问都是网络流
第二问的建图：
s - 1 - i (f[i]==1)
i - 1 - i'
i' - 1 - j (f[i]+1==f[j]&&a[i]<=a[j]&&i<j)
i' - 1 - t (f[i]==ans)
第三问只要把(s,1),(1,1'),(n,n'),(n',t)的流量改成INF
82分的原因是加边的时候忘记判a[i]>=a[j]
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
const int N=508;
int n,s,t,tmp;
int a[N];
int f[N],lis[N],num;
int ans;

int nume=1,head[N*2],cur[N*2];
struct node{
    int to,nxt,f;
}e[N*N*2];
inline void addedge(int x,int y,int z){
    e[++nume]=(node){y,head[x],z};head[x]=nume;
    e[++nume]=(node){x,head[y],0};head[y]=nume;
}

int dis[N*2];
int q[N*2],he,ta;
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
    for(int i=head[x];i;i=e[i].nxt){
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
inline void init()
{
    for(int i=0;i<=t;++i){
        for(int j=head[i];j;j=e[j].nxt){
            if(j%2==0){
                if((i==0&&e[j].to==1) || (i==1&&e[j].to==1+n) || (i==n&&e[j].to==n+n) || (i==n+n&&e[j].to==t)){
                    e[j].f=INT_MAX;
                    e[j^1].f=0;
                }
                else{
                    e[j].f+=e[j^1].f;
                    e[j^1].f=0;
                }
            }
        }
    }
}
int main()
{
    n=read();
    s=0;t=n+n+1;
    for(int i=1;i<=n;++i){
        a[i]=read();
    }
    
    for(int i=1;i<=n;++i){
        if(a[i]>=lis[num]){
            lis[++num]=a[i];
            f[i]=num;
        }
        else{
            tmp=upper_bound(lis+1,lis+num+1,a[i])-lis;
            lis[tmp]=a[i];
            f[i]=tmp;
        }
    }
    for(int i=1;i<=n;++i){
        ans=max(ans,f[i]);
    }
    printf("%d\n",ans);
    for(int i=1;i<=n;++i){
        addedge(i,i+n,1); 
        if(f[i]==1)	addedge(s,i,1);
        if(f[i]==ans)	addedge(i+n,t,1);
        for(int j=1;j<i;++j){
            if(f[i]==f[j]+1&&a[i]>=a[j]){
                addedge(j+n,i,1);
            }
        }
    }
    printf("%d\n",dinic());
    init();
    printf("%d\n",dinic());
    return 0;
}
```