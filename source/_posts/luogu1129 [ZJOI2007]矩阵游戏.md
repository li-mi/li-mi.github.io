---
title: luogu1129 [ZJOI2007]矩阵游戏
tag: 二分图
---
行变换和列变换不会使同一行或者同一列的东西分开
题目要求最后存在n个两两行列都不同的东西
那么一开始也要有n个两两行列都不同的东西
然后开始套路
左边n个点表示行
右边n个点表示列
(i,j)为1则 左i -> 右j
跑二分图匹配，为n则可以
<!--more-->
```cpp
#include<iostream>
#include<cstring>
#include<queue>
using namespace std;
const int N=408,M=100008;
int nume,head[N],from[M],to[M],f[M],nxt[M];
int d[N],gap[N],cur[N],p[N],num[N];
bool vis[N];
int t,n,k,en;
inline void addedge(int x,int y)
{
    ++nume;to[nume]=y;nxt[nume]=head[x];head[x]=nume;f[nume]=1;
    ++nume;to[nume]=x;nxt[nume]=head[y];head[y]=nume;f[nume]=0;
}
void bfs()
{
    queue<int> q;
    q.push(en);
    d[0]=en+1; 
    vis[en]=1;
    while(!q.empty()){
        int top=q.front();q.pop();
        for(int i=head[top];i;i=nxt[i]){
            if(!vis[to[i]]){
                vis[to[i]]=1;
                d[to[i]]=d[top]+1;
                q.push(to[i]);
            }
        }
    }
}
int augment()
{
    int x=en;
    while(x!=0){
        f[p[x]]--;
        f[p[x]^1]++;
        x=to[p[x]^1];
    }
    return 1;
}
int maxflow()
{
    bfs();
    int ans=0;
    for(int i=0;i<=en;i++)    num[d[i]]++;
    int x=0;
    for(int i=0;i<=en;i++){
        cur[i]=head[i];
    }
    while(d[0]<=en){
        
        if(x==en){
            ans+=augment();
            x=0;
        }    
        bool ok=0;
        for(int i=cur[x];i;i=nxt[i]){
            if(f[i]&&d[x]==d[to[i]]+1){
                ok=1;
                p[to[i]]=i;
                cur[x]=i;//cur[x]=i+1;
                x=to[i];
                break; 
            }
        }
        if(!ok){
            int m=en;
            for(int i=head[x];i;i=nxt[i]){
                if(f[i])    m=min(m,d[to[i]]);
            }
            num[d[x]]--;
            if(!num[d[x]])    break;
            d[x]=m+1;
            num[d[x]]++;
            cur[x]=head[x];
            if(x!=0)    x=to[p[x]^1];
        }
    }
    return ans;
}
int main()
{
    ios::sync_with_stdio(false);
    //cin.tie(0); 
    cin>>t; 
    while(t--){
        nume=1;
        memset(head,0,sizeof(head));
        memset(vis,0,sizeof(vis));
        memset(gap,0,sizeof(gap));
        cin>>n;
        en=(n<<1)+1;
        for(int i=1;i<=n;i++){
            for(int j=1;j<=n;j++){
                cin>>k;
                if(k)    addedge(i,j+n);
            }
        }
        for(int i=1;i<=n;i++)    addedge(0,i);
        for(int i=n+1;i<en;i++)    addedge(i,en);
        if(maxflow()==n)    cout<<"Yes"<<'\n';
        else    cout<<"No"<<'\n';
    }
    return 0;
}
```