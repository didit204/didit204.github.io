---
title: 城堡 The Castle 题解
date: 2026-08-15 09:02:39
tags:
---
题目地址: https://www.luogu.com.cn/problem/P1457#ide

{% raw %}
#include<iostream>
#include<vector>
using namespace std;
vector<int>pre(3010);//并查集
int h[4][2]={{0,-1},{-1,0},{0,1},{1,0}};
int find(int x){
    if(x==pre[x])return x;
    return pre[x]=find(pre[x]);
}
void join(int x,int y){
    pre[find(x)]=find(y);
}
int main(){
    int m,n;
    cin>>m>>n;
    int i,j;
    int s=n*m;
    for(i=1;i<=s;i++)pre[i]=i;
    for(i=1;i<=n;i++){
        for(j=1;j<=m;j++){
            int u;
            cin>>u;
            for(int k=0;k<4;k++){
                if(!(u&1))join((i-1)*m+j,(i+h[k][0]-1)*m+j+h[k][1]);
                u>>=1;
            }
        }
    }
    vector<int>v(3010);
    int c1=0,c2=0;
    for(i=1;i<=n;i++){
        for(j=1;j<=m;j++){
            v[find((i-1)*m+j)]++;
        }
    }
    for(i=1;i<=n;i++){
        for(j=1;j<=m;j++){
            if(v[(i-1)*m+j])c1++,c2=max(c2,v[(i-1)*m+j]);
        }
    }
    cout<<c1<<'\n'<<c2<<'\n';
    int ans=0;
    int x=0,y=0;
    char c='N';
        for(j=1;j<=m;j++){//从西到东
            for(i=n;i>=1;i--){//从南到北
            for(int k=1;k<=2;k++){
                int x1=i+h[k][0];
                int y1=j+h[k][1];
                if(x1<1||x1>n||y1<1||y1>m||find((i-1)*m+j)==find((x1-1)*m+y1))continue;
                int sum=v[find((i-1)*m+j)]+v[find((x1-1)*m+y1)];
                if(ans<sum){
                    ans=sum;
                    x=i;
                    y=j;
                    c=(k==1?'N':'E');
                }
            }
        }
    }
    cout<<ans<<'\n';
    cout<<x<<" "<<y<<" "<<c;
    return 0;
}
{% endraw %}
---