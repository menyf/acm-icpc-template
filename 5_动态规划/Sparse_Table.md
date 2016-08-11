# Sparse－Table算法

## 说明

复杂度分析

* 预处理 O(nlogn)
* 查询 O(1)

使用时请灵活处理，模版仅给出了最简单的按照区间最值的查询，还可以灵活的换为下标找下标在数组中的映射，举例：[LCA->RMQ](https://menyf.gitbooks.io/acm-icpc-template/content/7_%E5%9B%BE%E8%AE%BA/LCA/RMQ.html)

## 模版

### 最大/最小值查询
#### Tips
* 使用时请注意宏定义 RMQ
* `data[]`下标从1~n

#### 代码
```C++
#define RMQ max
const int maxn = 150005;//数据量
const int NVB = 33;//对于整型而言，其值不会超过2^32，因此第二维大小为33已经足够
int rmq[maxn][NVB];
void init(int data[],int n){
    /*data下标从1开始*/
    int k = log2(n);
    for(int i=1; i<=n; i++)
        rmq[i][0] = data[i];
    for(int j=1; j<=k; j++){
        for(int i=1; i+(1<<j)-1<=n; i++){
            rmq[i][j] = RMQ(rmq[i][j-1],rmq[i+(1<<(j-1))][j-1]);
        }
    }
}
int query(int l,int r){
    int k = log2(r-l+1);
    return RMQ(rmq[l][k],rmq[r-(1<<k)+1][k]);
}
```

### 二合一
#### Tips
* flag为1表示取最大值 0表示取最小值
* `data[]`下标从1~n

#### 代码
```C++
const int maxn = 150005;//数据量
const int NVB = 33;//对于整型而言，其值不会超过2^32，因此第二维大小为33已经足够
int mx[maxn][NVB],mn[maxn][NVB];
void init(int data[],int n){
    /*data下标从1开始*/
    int k = log2(n);
    for(int i=1; i<=n; i++)
        mx[i][0] = mn[i][0] = data[i];
    for(int j=1; j<=k; j++){
        for(int i=1; i+(1<<j)-1<=n; i++){
            mx[i][j] = max(mx[i][j-1],mx[i+(1<<(j-1))][j-1]);
            mn[i][j] = min(mn[i][j-1],mn[i+(1<<(j-1))][j-1]);
        }
    }
}
int query(int l,int r,int flag){
    int k = log2(r-l+1);
    if(flag) return max(mx[l][k],mx[r-(1<<k)+1][k]);
    else return min(mn[l][k],mn[r-(1<<k)+1][k]);
}
```