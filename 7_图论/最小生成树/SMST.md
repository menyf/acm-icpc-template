# 次小生成树

## 说明
求最小生成树时，用数组maxd[i][j]来表示MST中i到j最大边权，求完后，直接枚举所有不在MST中的边，替换掉最大边权的边，更新答案

## 使用方法
1. `SMST::init(n);`
2. `SMST::adde(u, v, w);` 初始化顶点个数，起始点位置
3. `int status = SMST::smst();` 返回-1表示无最小生成树，返回-2表示有最小生成树无次小生成树，否则返回次小生成树的权

## Tips
* `SMST::weight`直接记录了最小生成树的权重
* 对于平行边/自环要进行特殊处理 

## 模版
```C++
// 顶点下标1～n
const int Vmax = 1005;
namespace SMST{
    // MST
    int n;//n个顶点
    int s;//起点 树的根
    int weight; //MST的重量
    int mp[Vmax][Vmax];
    int dis[Vmax];  //dis[i]表示指向i点的最短的边
    bool vis[Vmax]; //标记该点是否在树上
    int pre[Vmax];//记录前驱
    
    //SMST
    int subweight; //SMST的重量
    bool used[Vmax][Vmax];  //表示当前边有没有被用过
    int maxd[Vmax][Vmax];   //maxd[u][v]表示 u-v路径上最大的边权
    
    void init(int Vsz,int source=1){//默认起点为1
        memset(mp, INF, sizeof(mp));
        memset(dis, INF, sizeof(dis));
        memset(used, 0, sizeof(used));
        memset(maxd, 0, sizeof(maxd));
        memset(vis, false, sizeof(vis));
        n=Vsz;
        for (int i=0; i<=n; i++) {
            mp[i][i] = 0;
        }
        weight=0;
        dis[source] = 0;
        pre[source] = source;
    }
    
    //1~n的邻接矩阵
    void adde(int u,int v,int w){
        mp[u][v]=w;
        mp[v][u]=w;
    }
    
    int prim(){
        /*
         返回值说明：
         -1: 无生成树
         weight: 最小生成树的重量
         */
        
        for(int i=1;i<=n;i++){
            int pos=0;
            int minc = INF;
            for(int j=1;j<=n;j++){
                if(!vis[j]&&dis[j]<minc){
                    pos=j;
                    minc = dis[j];
                }
            }
            if(minc == INF) return -1; //n个点不联通 无生成树

            weight+=dis[pos];
            used[pos][pre[pos]] = true; // for SMST
            used[pre[pos]][pos] = true; // for SMST
            vis[pos]=true;

            for(int j=1;j<=n;j++){
                if (j == pos) continue;
                if(vis[j]) {
                    maxd[j][pos] = maxd[pos][j] = max(dis[pos], maxd[j][pre[pos]]);
                }
                else if(!vis[j]&&mp[pos][j]<dis[j]){
                    dis[j]=mp[pos][j];
                    pre[j]=pos;
                }
            }
            
        }
        return weight;
    }
    
    int smst(){
        /*
         返回值说明：
         -1: 无生成树
         -2: 有最小生成树 无次小生成树 （比如给出的图即为一棵树）
         subweight: 次小生成树的重量
         */
        if(prim() == -1) return -1; //无生成树
        subweight = INF;
        for(int i = 1; i<= n; i++){
            for(int j = i + 1; j <= n; j++){
                if(mp[i][j]!=INF && !used[i][j]){
                    subweight = min(subweight, weight+mp[i][j]-maxd[i][j]);
                }
            }
        }
        if(subweight == INF) return -2; //只有唯一生成树 也就是说只有最小生成树 没有次小生成树
        return subweight;
    }
}
```