# Tarjan

## 说明
离线的方式进行查询，先将所有询问录入，进行DFS搜索时处理询问，使用并查集的思想。

复杂度O(n+q)

## 使用方法
1. 见主函数

## Tips
* 不推荐使用该方法查询LCA，**推荐使用倍增法**

## 模版
```C++
#include <algorithm>
#include <cmath>
#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <ctime>
#include <ctype.h>
#include <iostream>
#include <map>
#include <queue>
#include <set>
#include <stack>
#include <string>
#include <vector>
#define eps 1e-8
#define INF 0x7fffffff
#define PI acos(-1.0)
#define seed 31//131,1313
typedef long long LL;
typedef unsigned long long ULL;
using namespace std;
const int maxn = 300000+10;
int pre[maxn],point[maxn],point2[maxn];
bool vis[maxn];
struct Edge
{
    int v;///连接点
    int next;///下一条从此边的出发点发出的边
} edge[maxn*2];
struct Query
{
    int v;
    int w;
    int next;
} query[maxn*2];
int top,top2;
void init()
{
    memset(vis,0,sizeof(vis));
    memset(point,-1,sizeof(point));
    memset(point2,-1,sizeof(point2));
    top=0;
    top2=0;
}
void add_edge(int u,int v)
{
    edge[top].v=v;
    edge[top].next=point[u];///上一条边的编号
    point[u]=top++;///u点的第一条边编号变成head
}
void findset(int x) ///并查集
{
    if(x!=pre[x])
    {
        pre[x]=findset(pre[x]); ///路径压缩
    }
    return pre[x];
}
void add_query(int u,int v)
{
    query[top2].v=v;
    query[top2].w=-1;
    query[top2].next=point2[u];///上一条边的编号
    point2[u]=top2++;///u点的第一条边编号变成head
    query[top2].v=u;
    query[top2].w=-1;
    query[top2].next=point2[v];///上一条边的编号
    point2[v]=top2++;///u点的第一条边编号变成head
}
int lca(int u,int f) ///当前节点，父节点
{
    pre[u]=u;  ///设立当前节点的集合
    for(int i=point[u]; i!=-1; i=edge[i].next)
    {
        if(edge[i].v==f)
            continue;
        lca(edge[i].v,u);  ///搜索子树
        pre[edge[i].v]=u; ///合并子树
    }
    vis[u]=1; ///以u点为集合的点搜索完毕
    for(int i=point2[u]; i!=-1; i=query[i].next)
    {
        if(vis[query[i].v]==1)
            query[i].w=findset(query[i].v);
    }
    return 0;
}
int main()
{
    int root[maxn];
    int T;
    scanf("%d",&T);
    for(int ii=0; ii<T; ii++)
    {
        init();
        int tot,r=-1,a,b;
        scanf("%d",&tot);
        for(int i=1; i<=tot; i++)
            root[i]=i;
        for(int i=0; i<tot-1; i++)
        {
            scanf("%d%d",&a,&b);
            add_edge(a,b);
            add_edge(b,a);
            root[b]=a;
        }
        for(int i=1; i<=tot; i++)
            if(root[i]==i)
                r=i;///树的根
        scanf("%d%d",&a,&b);
        add_query(a,b);
        lca(r,r);
        for(int i=0;i<top2;i++)
        {
            if(query[i].w!=-1)
                printf("%d\n",query[i].w);
        }
        
    }
    return 0;
}

```