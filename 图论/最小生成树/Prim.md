# Prim

## 说明
找对于到每一个点最近边的长度，存在对应下标的dis数组中

稀疏图较快

## 使用方法
1. `Prim p;`
2. `p.init(Vsz, source);` 初始化顶点个数，起始点位置
3. `p.adde(u, v, w);` 从u指向v权值为w的边，加一次边即可
4. `int weight = p.prim(); `输出最小生成树的重量，无法建成则返回-1

## Tips
最好用Prim_Heap，相同条件下更快

考虑数据范围，树的重量可能会爆int

## 模版
```C++
#define Vmax 999
#define INF 0x3f3f3f3f
struct Prim{
    int n;//n个顶点
    int s;//起点 树的根
    int weight;//树的重量
    int mp[Vmax][Vmax];
    int dis[Vmax];  //dis[i]表示指向i点的最短的边
    bool vis[Vmax];
    int pre[Vmax];//记录前驱
    
    void init(int Vsz,int source=1){//默认起点为1
        n=Vsz;
        weight=0;
        for(int i=0;i<=n;i++){
            dis[i]=INF;
            vis[i]=false;
        }
        dis[source]=0;
    }
    
    //1~n的邻接矩阵
    void adde(int u,int v,int w){
        mp[u][v]=w;
        mp[v][u]=w;
    }
    
    int prim(){
        int cnt=0;
        for(int i=1;i<=n;i++){
            int pos=0;
            for(int j=1;j<=n;j++){
                if(!vis[j]&&dis[j]<dis[pos])
                    pos=j;
            }
            vis[pos]=true;cnt++;
            weight+=dis[pos];
            for(int j=1;j<=n;j++){
                if(!vis[j]&&mp[pos][j]<dis[j]){
                    dis[j]=mp[pos][j];
                    pre[j]=pos;
                }
            }
        }
        if(cnt==n) return weight;
        else return -1;
    }
};
```