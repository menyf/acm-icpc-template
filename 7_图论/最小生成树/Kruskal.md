# Kruskal

## 说明
结构体排序＋并查集———结构体排序找到所有符合条件的边，加入并查集

稀疏图较快

## 使用方法
1. `Kruskal d;`
2. `d.init(Vsz, Esz);` 初始化顶点个数，边的个数
3. `d.adde(u, v, w);` 从u指向v权值为w的边，单向加边即可
4. `int weight = d.kruskal(); `输出最小生成树的重量，无法建成则返回-1

## Tips
待写

## 模版
```C++
#include <algorithm>
using namespace std;
#define Vmax 300    //最大点数量
#define Emax 1000   //最大边数量
struct Kruskal{
    int n,m;//n个点 m条边
    int rank[Vmax],fa[Vmax];//并查集需要
    int weight;// weight为最小生成树的重量
    int ecnt;
    
    struct edge{
        int u,v,w;//起点 终点 权值
        bool operator<(const edge e) const{
            return w<e.w;
        }
    }e[Emax];
    
    void init(int Vsz,int Esz){
        n=Vsz;m=Esz;
        ecnt=0;
        weight=0;
        for(int i=0;i<=Vsz;i++){
            fa[i]=i;
            rank[i]=0;
        }
    }
    
    //边的编号从0～m-1
    void adde(int u,int v,int w){
        e[ecnt].u=u;
        e[ecnt].v=v;
        e[ecnt++].w=w;
    }
    
    int findx(int x){
        while(x!=fa[x])
            x=fa[x];
        return x;
    }
    
    void set_merge(int u,int v){
        if(rank[u]<rank[v]){
            fa[u]=v;
            rank[v]+=rank[u];
        }
        else{
            fa[v]=u;
            rank[u]+=rank[v];
        }
    }
    
    int kruskal(){
        int cnt=0;
        sort(e,e+m);
        for(int i=0;i<m;i++){
            int uR=findx(e[i].u);
            int vR=findx(e[i].v);
            if(uR!=vR){
                set_merge(uR, vR);
                weight+=e[i].w;
                cnt++;
            }
        }
        if(cnt==n-1) return weight;//最小生成树可建
        else return -1; //最小生成树不可建
    }
};
```