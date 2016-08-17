# Dinic

## 说明

## 使用方法
1. `Dinic d;`
2. `d.init(start,end);` 初始化
3. ` d.adde(x,y,c);` 加边
4. `cout<<d.dinic<<endl`输出最大流的值

## Tips
* 较EdmondsKarp快一点，但是最好用ISAP
* adde是加一条有向边，体现在参量网络上是加了正向反向两条边，如果原图中是无向的，应该执行两次加边过程

## 示例
* 网络流最大权闭合图[POJ 2987 Firing](http://menyf.github.io/2016/08/17/poj2987/)

## 模版
```C++
#define vmax 20005  //最大顶点数
#define emax 500005 //最大边数
struct Dinic{
    int src,target,ecnt;//源点，汇点，边数
    int head[vmax];//邻接表表头
    int cur[vmax];
    int dis[vmax];//从源点到该点的距离
    struct edge{
        int to,next,cap;
    }e[2*emax];//边可能是双向的，故乘2
    
    void init(int start,int end){
        ecnt=0;
        src=start;
        target=end;
        memset(head,-1,sizeof(head));
    }
    
    void adde(int from,int to,int cap){
        e[ecnt].to=to;
        e[ecnt].cap=cap;
        e[ecnt].next=head[from];
        head[from]=ecnt++;
        
        e[ecnt].to=from;
        e[ecnt].cap=0;
        e[ecnt].next=head[to];
        head[to]=ecnt++;
    }
    
    bool BFS(){
        memset(dis, -1, sizeof(dis));
        dis[src]=0;
        queue<int>q;
        q.push(src);
        while(!q.empty()){
            int tmp=q.front();
            q.pop();
            for(int i=head[tmp];i!=-1;i=e[i].next){
                int tp=e[i].to;
                if(dis[tp]==-1&&e[i].cap){
                    dis[tp]=dis[tmp]+1;
                    q.push(tp);
                    if(tp==target)
                        return true;
                }
            }
        }
        return false;
    }
    
    int DFS(int u,int delta){
        if(u==target||delta==0)
            return delta;
        int f,flow=0;
        for(int& i=cur[u];i!=-1;i=e[i].next){
            if(dis[u]+1==dis[e[i].to] && (f=DFS(e[i].to,min(delta,e[i].cap))))
            {
                e[i].cap-=f;
                e[i^1].cap+=f;
                flow+=f;
                delta-=f;
                if(!delta)break;
            }
        }
        return flow;
    }
    
    int dinic(){
        int tmp=0,maxflow=0;
        while(BFS()){
           for(int i=src;i<=target;i++) cur[i]=head[i];
            while(tmp=DFS(src, INF))
                maxflow+=tmp;
        }
        return maxflow;
    }
};
```