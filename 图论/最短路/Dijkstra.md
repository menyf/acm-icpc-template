# Dijkstra

## 说明
Dijkstra_Heap

## 使用方法
1. `Dijkstra d;`
2. `d.init(n);` 顶点为n
3. ` d.adde(x,y,w);` 加边
4. `cout<<d.get_ans(int source,int destination)<<endl`输出最短路的值

## Tips
较普通的Dijkstra会快一些，**最快的还是SPFA**

## 模版
```C++
const int maxn=100000+5; //最大顶点数
const int INF=0x3f3f3f3f;
using namespace std;
struct Edge{
    int from,to, dist;
    Edge(int u,int v,int d):from(u),to(v),dist(d){}
};
struct HeapNode{
    int d,u;
    bool operator < (const HeapNode& rhs)const{
        return d>rhs.d;
    }
};
struct Dijkstra{
    int n,m;
    vector<Edge> edges;
    vector<int> G[maxn];
    bool done[maxn];        //是否已永久标号
    int d[maxn];            //s(源点)到各个点的距离
    int p[maxn];            //最短路中的上一条弧
    
    void init(int n){
        this->n=n;
        for(int i=0;i<=n;i++) G[i].clear();
        edges.clear();
    }
    
    void adde(int from,int to,int dist){
        edges.push_back(Edge(from-1, to-1, dist));
        m=edges.size();
        G[from-1].push_back(m-1);
    }
    
    void dijkstra(int s){
        priority_queue<HeapNode> q;
        for(int i=0;i<n;i++) d[i]=INF;
        d[s]=0;
        memset(done, 0, sizeof(done));
        q.push((HeapNode){0,s});
        while(!q.empty()){
            HeapNode x=q.top();q.pop();
            int u=x.u;
            if(done[u]) continue;
            done[u]=true;
            for(int i=0;i<G[u].size();i++){
                Edge& e=edges[G[u][i]];
                if(d[e.to]>d[u]+e.dist){
                    d[e.to]=d[u]+e.dist;
                    p[e.to]=G[u][i];
                    q.push((HeapNode){d[e.to],e.to});
                }
            }
        }
    }
    
    int get_ans(int source,int destination){
        dijkstra(source-1);
        return d[destination-1];
    }
};
```