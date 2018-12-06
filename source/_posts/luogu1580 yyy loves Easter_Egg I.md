---
title: luogu1580 yyy loves Easter_Egg I
tag: 模拟
---
```
坑在于输入
windows下每行结束有两个字符，为\r和\n
linux下只有\n
以及被钦点的人只要吱声就算成功油炸，不管他是不是@自己
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
int n,num,cnt;
string s,name,name2;
inline string get(int x)
{
    string tmp="";
    for(int i=x;;++i){
        if(s[i]=='\0'||s[i]==' '){
            return tmp;
        }	
        tmp+=s[i];
    }
}
inline string get_at_name()
{
    for(int i=0;i<n;++i){
        if(s[i]=='@'){
            return get(i+11);
        }
    }
}
inline string get_his_name()
{
    return get(10);
}
int main()
{
    getline(cin,s,'\r');getchar();//getchar()处理掉'\n'
    ++num;
    n=s.size();
    name=get_at_name();
    while(1){
        getline(cin,s,'\r');getchar();
        if(s==""){
            break;
        }
        ++num;
        n=s.size();
        cnt=0;
        for(int i=0;i<n;++i){
            if(s[i]=='@'){
                ++cnt;
            }
        }
		name2=get_his_name();
        if(name2==name){
            cout<<"Successful @yyy loves "<<name<<" attempt\n";
        }
        if(cnt!=1||name!=get_at_name()){
            cout<<"Unsuccessful @yyy loves "<<name<<" attempt\n";
            cout<<num<<'\n';
            cout<<"yyy loves "<<name2<<'\n';
            return 0;
        }
    }
    cout<<"Unsuccessful @yyy loves "<<name<<" attempt\n";
    cout<<num<<'\n';
    puts("Good Queue Shape");
    return 0;
}
```