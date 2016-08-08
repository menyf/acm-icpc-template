# Prim_Heap

## 说明
找对于到每一个点最近边的长度，存在对应下标的dis数组中

稀疏图较快

## 使用方法
1. 见`init()`函数
2. `mytype ans = prim_heap(s)` 得到以s为起点开始找的最小生成树重量
3. `judge(n)`判断该树能否生成

## Tips
最小生成树无脑情况就用这个

考虑数据范围，树的重量可能会爆int

## 模版
```C++
typedef int mytype;
const int NV=105;
const int NE=10005*2;
mytype dis[NV];
int pre[NV],vis[NV],he[NV],ecnt,pcnt;
struct edge
{
    int v,next;
    mytype l;
} E[NE];
void adde(int u,int v,mytype l)
{
    E[++ecnt].v=v;
    E[ecnt].l=l;
    E[ecnt].next=he[u];
    he[u]=ecnt;
}
void init(int n,int m,int s)
{
    ecnt=0;
    memset(pre,0,sizeof(pre));
    memset(vis,0,sizeof(vis));
    memset(he,-1,sizeof(he));
    for (int i=0; i<=n; i++)
        dis[i]=inf;
    dis[s]=0;
    for (int i=1; i<=m; i++)
    {
        int u,v;
        mytype l;
        scanf("%d%d%d",&u,&v,&l);
        adde(u,v,l);
        adde(v,u,l);
    }
}
struct point
{
    int u;
    mytype l;
    point(int u,mytype l):u(u),l(l) {}
    bool operator<(const point &p) const
    {
        return l>p.l;
    }
};
mytype prim_heap(int s)
{
    priority_queue<point> q;
    q.push(point(s,0));
    mytype ans=0;
    pcnt=0;
    while(!q.empty())
    {
        point p=q.top();
        q.pop();
        int u=p.u;
        if (vis[u])
            continue;
        vis[u]=1;
        ans+=p.l;//==dis[x]
        pcnt++;
        for (int i=he[u]; i!=-1; i=E[i].next)
            if (!vis[E[i].v]&&E[i].l<dis[E[i].v])
            {
                dis[E[i].v]=E[i].l;
                pre[E[i].v]=u;
                q.push(point(E[i].v,dis[E[i].v]));
            }
    }
    return ans;
}
bool judge(int n)
{
    return pcnt==n;
}

```