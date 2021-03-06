# EdmondsKarp

说明：

* n为定点数量
* m为边数乘2
* e[]和g[][]存储邻接表
* a[]用于记录通过当前边的最大流量
* p[]记录路径

使用方法：

1. `EdmondsKarp X;`
2. `X.init(n);`	$$n$$为顶点数
3. `X.AddEdge(from, to, cap);` 加边
4. `int ans = X.Maxflow(start, sink);`	求出最大流

Tips：

复杂度较高，慎用

```C++
#define maxn 10005//表示节点数量
struct edge{
    int from,to,cap,flow;
    edge(int u,int v,int c,int f):from(u),to(v),cap(c),flow(f){}
};
struct EdmondsKarp{
    int n,m;
    vector<edge>e;
    vector<int>g[maxn];
    int a[maxn];
    int p[maxn];
    void init(int n){
        for(int i=0;i<n;i++)
            g[i].clear();
        e.clear();
    }
    
    void AddEdge(int from,int to,int cap){
        e.push_back(edge(from, to, cap, 0));
        e.push_back(edge(to, from, 0, 0));
        m=e.size();
        g[from].push_back(m-2);
        g[to].push_back(m-1);
    }
    
    int Maxflow(int s,int t){//source target
        int flow=0;
        while(1){
            memset(a, 0, sizeof(a));
            queue<int>q;
            q.push(s);
            a[s]=INF;
            while(!q.empty()){
                int tmp=q.front();q.pop();
                for (int i=0; i<g[tmp].size(); i++){
                    edge& etmp=e[g[tmp][i]];
                    if(!a[etmp.to]&&etmp.cap>etmp.flow){
                        p[etmp.to]=g[tmp][i];
                        a[etmp.to]=min(a[tmp], etmp.cap-etmp.flow);
                        q.push(etmp.to);
                    }
                }
                if(a[t]) break;
            }
            if(!a[t])break;
            for(int u=t;u!=s;u=e[p[u]].from){
                e[p[u]].flow+=a[t];
                e[p[u]^1].flow-=a[t];
            }
            flow+=a[t];
        }
        return flow;
    }
};
```