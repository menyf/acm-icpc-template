# SPFA

## 说明
SPFA是Bellman-Ford的进化版

SPFA在网络流中也有应用

可以用于判环，同样可以判环的还有拓扑排序

## 使用方法
1. 确定点边的最大数量 Vmax Emax
2. `init()`
3. `adde(x,y,w);adde(y,x,w);` **双向加边**
4. `cout<<get_ans(int n,int source,int destination)<<endl` n个点、起点、终点

## Tips
想到再写

## 模版
```C++
#define Vmax 500
#define Emax 1000
const int INF=0x3f3f3f3f;
int pre[Vmax],ecnt;
struct edge{
    int v,w,next;
}e[Emax*2];
int dis[Vmax];
int vcnt[Vmax];//记录每个点进队次数，用于判断是否出现负环
bool inq[Vmax];
void init(){
    ecnt=0;
    memset(vcnt,0,sizeof(vcnt));
    memset(pre,-1,sizeof(pre));
    memset(inq, false, sizeof(inq));
}
//***注意双向加边
void adde(int from,int to,int w){
    e[ecnt].v=to;
    e[ecnt].w=w;
    e[ecnt].next=pre[from];
    pre[from]=ecnt++;
}
bool SPFA(int n,int source){//n为顶点数 source为起点
                            //return true表示无负环，反之亦然
    for (int i=0; i<=n; i++)
        dis[i]=INF;
    dis[source]=0;
    queue<int>q;
    q.push(source);inq[source]=true;
    
    while (!q.empty()) {
        int tmp=q.front();
        q.pop();inq[tmp]=false;
        
        //判断负环
        vcnt[tmp]++;
        if (vcnt[tmp]>=n) return false;
        
        for (int i=pre[tmp]; i!=-1; i=e[i].next) {
            int w=e[i].w;
            int v=e[i].v;
            if (dis[tmp]+w<dis[v]) {
                dis[v]=dis[tmp]+w;  //松弛操作
                if (!inq[v]) {
                    q.push(v);
                    inq[v]=true;
                }
            }
        }
    }
    return true;
}
int get_ans(int n,int source,int destination){
    SPFA(n,source);
    return dis[destination];
}
```