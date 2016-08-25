# RMQ

## 说明
RMQ：Range Minimum(Maximum) Query

* `sum[]`为线段树，需要开辟四倍的元素数量的空间。
* `build()`为建树操作
* `update()`为更新操作
* `query()`为查询操作

## 使用方法
1. 根据情况修改RMQ的宏定义
2. `build(1, n);` 建立一个叶子节点为`n`个的线段树
3. `update(pos, val, 1, n);` 修改树中下标为`pos`的叶子节点值为`val`
4. `query(l, r, 1, n);` 查询`[l ,r]`区间中的RMQ

## Tips
* 建出来的树为空树，默认每个点值都为0，需要自行将值update上去，或者修改build中`sum[rt]=0;`为输入操作`scanf("%d",sum+rt);`
* RMQ为宏定义，请根据情况自行修改为`max`或者`min`，对应修改`query`中的`res`为`-INF`或者`INF`

## 模版
```C++
const int maxn=2005+5;
#define RMQ max
#define lson l,m,rt<<1
#define rson m+1,r,rt<<1|1
int sum[maxn<<2]={};
void pushup(int rt){
    sum[rt]=RMQ(sum[rt<<1],sum[rt<<1|1]);
}
void build(int l,int r,int rt=1){
    if (l==r){
        sum[rt]=0;
        return;
    }
    int m=(l+r)>>1;
    build(lson);
    build(rson);
    pushup(rt);
}
void update(int pos,int val,int l,int r,int rt=1){
    if (l==r) {
        sum[rt]=val;
        return;
    }
    int m=(l+r)>>1;
    if (pos<=m) update(pos, val, lson);
    else update(pos, val, rson);
    pushup(rt);
}
int query(int L,int R,int l,int r,int rt=1){
    if (L<=l&&r<=R) return sum[rt];
    int m=(l+r)>>1;
    int res=-INF;	//防负数的坑
    if (L<=m) res=RMQ(res,query(L, R, lson));
    if (R>m)  res=RMQ(res,query(L, R, rson));
    return res;
}
```

