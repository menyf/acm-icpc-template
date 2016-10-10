# Hopcroft-Karp
## 说明
二分图最大匹配 Hopcroft-Karp算法

二分图集合X和Y。求X的最大匹配

复杂度：O (E * sqrt(V))

不停的BFS求出距离标号，然后DFS找增广路

适用于数据较大的二分匹配

## 使用方法
1. nx, ny需要初始化
2. `g[i].clear() `对于 0<=i<=nx
2. `g[x].pb(y);`表示加一条从x指向y的边，注意单向加边
3. `int ans = match();` 得到最大匹配数

## Tips
* 下标从1~nx以及1~ny

## 模版
```C++
const int maxn = 500;
int nx, ny; //nx表示X部的点，ny表示Y部的点
vector<int> g[maxn];
int mx[maxn], my[maxn]; //mx表示X部中每个点对应的y值，my表示Y部中每个点对应的x值
queue<int> q;
int dx[maxn], dy[maxn];
bool vis[maxn];
bool find (int u) {
    for (int i = 0; i < g[u].size(); i++) if (!vis[g[u][i]] && dy[g[u][i]] == dx[u] + 1) {
        int v = g[u][i];
        vis[v] = true;
        if (!my[v] || find(my[v])) {
            mx[u] = v;
            my[v] = u;
            return true;
        }
    }
    return false;
}
int match() {
    memset(mx, 0, sizeof(mx));
    memset(my, 0, sizeof(my));
    int ans = 0;
    while(true) {
        bool flag = false;
        while(!q.empty()) q.pop();
        memset(dx, 0, sizeof(dx));
        memset(dy, 0, sizeof(dy));
        for (int i = 1; i <= nx; i++) if (!mx[i]) q.push(i);
        while(!q.empty()) {
            int u = q.front();
            q.pop();
            for (int i = 0; i < g[u].size(); i++) if (!dy[g[u][i]]) {
                int v = g[u][i];
                dy[v] = dx[u] + 1;
                if (my[v]) {
                    dx[my[v]] = dy[v] + 1;
                    q.push(my[v]);
                }
                else flag = true;
            }
        }
        if (!flag) break;
        memset(vis, 0, sizeof(vis));
        for (int i = 1; i <= nx; i++) if (!mx[i] && find(i)) ans++;
    }
    return ans;
}
```