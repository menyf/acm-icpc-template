# 2-SAT

## 说明
### 学习资料
* [2-SAT问题——饶齐](http://blog.csdn.net/u013480600/article/details/44858849)
* [2-SAT总结——kuangbin](http://www.cnblogs.com/kuangbin/archive/2012/10/05/2712429.html)
* 白书

## 个人理解



## 使用方法


## Tips

## 模版
```C++
const int maxn=10000+10;
struct TwoSAT
{
    int n;//原始图的节点数(未翻倍)
    vector<int> G[maxn*2];//G[i]==j表示如果mark[i]=true,那么mark[j]也要=true
    bool mark[maxn*2];//标记
    int S[maxn*2],c;//S和c用来记录一次dfs遍历的所有节点编号

    void init(int n)
    {
        this->n=n;
        for(int i=0;i<2*n;i++) G[i].clear();
        memset(mark,0,sizeof(mark));
    }

    //加入(x,xval)或(y,yval)条件
    //xval=0表示假，yval=1表示真
    void add_clause(int x,int xval,int y,int yval)
    {
        x=x*2+xval;
        y=y*2+yval;
        G[x^1].push_back(y);
        G[y^1].push_back(x);
    }

    //从x执行dfs遍历,途径的所有点都标记
    //如果不能标记,那么返回false
    bool dfs(int x)
    {
        if(mark[x^1]) return false;//这两句的位置不能调换
        if(mark[x]) return true;
        mark[x]=true;
        S[c++]=x;
        for(int i=0;i<G[x].size();i++)
            if(!dfs(G[x][i])) return false;
        return true;
    }

    //判断当前2-SAT问题是否有解
    bool solve()
    {
        for(int i=0;i<2*n;i+=2)
        if(!mark[i] && !mark[i+1])
        {
            c=0;
            if(!dfs(i))
            {
                while(c>0) mark[S[--c]]=false;
                if(!dfs(i+1)) return false;
            }
        }
        return true;
    }
};

```