# ISAP

## 说明
ISAP + Gap优化

## 使用方法
1. `ISAP::init(st, ed, n);`初始化起点、终点、顶点数
2. `ISAP::adde(u, v, cap);ISAP::adde(v, u, cap);`双向加边
3. `int ans = ISAP::maxflow();`得到结果

## Tips
1. 该模版只能跑一次网络流
2. 注意Vmax和Emax

## 模版
```C++
const int INF=0x3f3f3f3f;
namespace ISAP{
    const int Vmax = 420900;
    const int Emax = 420900;
    int n;
    struct Node {
        int v;    // vertex
        int cap;    // capacity
        int flow;   // current flow in this arc
        int nxt;
    } e[Emax * 2];
    int g[Vmax], fcnt;
    int st, ed;
    int dist[Vmax], numbs[Vmax], q[Vmax];
    void adde(int u, int v, int c) {    //单向加边的过程
        e[++fcnt].v = v;
        e[fcnt].cap = c;
        e[fcnt].flow = 0;
        e[fcnt].nxt = g[u];
        g[u] = fcnt;
        e[++fcnt].v = u;
        e[fcnt].cap = 0;
        e[fcnt].flow = 0;
        e[fcnt].nxt = g[v];
        g[v] = fcnt;
    }
    void init(int src,int sink,int n_) {
        memset(g, 0, sizeof(g));
        fcnt = 1;
        n=n_;
        st = src, ed = sink;/*修改*/
    }
    void rev_bfs() {
        int font = 0, rear = 1;
        for (int i = 0; i <= n; i++) { //n为总点数
            dist[i] = Vmax;
            numbs[i] = 0;
        }
        q[font] = ed;
        dist[ed] = 0;
        numbs[0] = 1;
        while(font != rear) {
            int u = q[font++];
            for (int i = g[u]; i; i = e[i].nxt) {
                if (e[i ^ 1].cap == 0 || dist[e[i].v] < Vmax) continue;
                dist[e[i].v] = dist[u] + 1;
                ++numbs[dist[e[i].v]];
                q[rear++] = e[i].v;
            }
        }
    }
    int maxflow() {
        rev_bfs();
        int u, totalflow = 0;
        int curg[Vmax], revpath[Vmax];
        for(int i = 0; i <= n; ++i) curg[i] = g[i];
        u = st;
        while(dist[st] < n) {
            if(u == ed) {   // find an augmenting path
                int augflow = INF;
                for(int i = st; i != ed; i = e[curg[i]].v)
                    augflow = min(augflow, e[curg[i]].cap);
                for(int i = st; i != ed; i = e[curg[i]].v) {
                    e[curg[i]].cap -= augflow;
                    e[curg[i] ^ 1].cap += augflow;
                    e[curg[i]].flow += augflow;
                    e[curg[i] ^ 1].flow -= augflow;
                }
                totalflow += augflow;
                u = st;
            }
            int i;
            for(i = curg[u]; i; i = e[i].nxt)
                if(e[i].cap > 0 && dist[u] == dist[e[i].v] + 1) break;
            if(i) {   // find an admissible arc, then Advance
                curg[u] = i;
                revpath[e[i].v] = i ^ 1;
                u = e[i].v;
            } else {    // no admissible arc, then relabel this vertex
                if(0 == (--numbs[dist[u]])) break;    // GAP cut, Important!
                curg[u] = g[u];
                int mindist = n;
                for(int j = g[u]; j; j = e[j].nxt)
                    if(e[j].cap > 0) mindist = min(mindist, dist[e[j].v]);
                dist[u] = mindist + 1;
                ++numbs[dist[u]];
                if(u != st)
                    u = e[revpath[u]].v;    // Backtrack
            }
        }
        return totalflow;
    }
}
```