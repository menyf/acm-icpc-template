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
* `pre[]`数组未验证，请使用时注意

## 模版
```C++
const int INF=0x3f3f3f3f;
typedef int mytype;
const int Vmax = 500+10;
const int Emax = 1000+10;//未翻倍时
int he[Vmax],ecnt;
struct edge{
	int v,next;
	mytype w;
}e[Emax*2];
int dis[Vmax];
int vcnt[Vmax]; //记录每个点进队次数，用于判断是否出现负环
bool inq[Vmax];
int pre[Vmax];  //记录最短路中的上一个节点
void init(){
	// 邻接表初始化
	ecnt=0;
	memset(he,-1,sizeof(he));
	
	// SPFA初始化
	memset(inq, false, sizeof(inq));
	memset(vcnt,0,sizeof(vcnt));
	memset(dis, INF, sizeof(INF));	//即使是dis类型为long long也可赋int型的INF
}
//***注意双向加边
void adde(int from,int to,mytype w){
	e[ecnt].v=to;
	e[ecnt].w=w;
	e[ecnt].next=he[from];
	he[from]=ecnt++;
}
bool SPFA(int n,int source){//n为顶点数 source为起点
	//return true表示无负环，反之亦然
	dis[source]=0;
	queue<int>q;
	q.push(source);inq[source]=true;

	while (!q.empty()) {
		int tmp=q.front();
		q.pop();inq[tmp]=false;

		//判断负环
		vcnt[tmp]++;
		if (vcnt[tmp]>=n) return false;

		for (int i=he[tmp]; i!=-1; i=e[i].next) {
			int w=e[i].w;
			int v=e[i].v;
			if (dis[tmp]+w<dis[v]) {
				dis[v]=dis[tmp]+w;  //松弛操作
				pre[v]=tmp;
				if (!inq[v]) {
					q.push(v);
					inq[v]=true;
				}
			}
		}
	}
	return true;
}
mytype get_ans(int n,int source,int destination){
	SPFA(n,source);
	return dis[destination];
}
```