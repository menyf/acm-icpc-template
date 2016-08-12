# Trie前缀树
## 说明
此模版用于查找对于若干个字符串，查找某字符串作为前缀出现的次数

* `maxNode` ：最大节点的数量
* `sigma_size`：每个节点的子节点个数
* `sz` ：表示字典树中下一个节点的下标，根节点编号为0，不指向任何字符
* `val[]` ：到当前位置的前缀串出现的次数

## 使用方法
1. `init();`初始化
2. `insert()`插入所有字符串
3. `query()`查询

## 模版
```C++
const int maxNode=1005*25;
const int sigma_size=26;
int c[maxNode][sigma_size];
//    int val[maxNode];  //val[i]用于记录以下标以i结尾的辅助信息，如权值
int cnt[maxNode];
int sz;
void init()
{
    sz=1;
    memset(c[0],0,sizeof(c[0]));
    memset(cnt,0,sizeof(cnt));
}
int idx(char ch){
    return ch - 'a';
}
void insert(char s[],int v=0)	//v表示该字符串的辅助信息，如权值等，使用时需要取消对应注释
{
    int u=0;
    for (int i=0; s[i]; i++)
    {
        char ch = idx(s[i]);
        if (!c[u][ch])
        {
            memset(c[sz],0,sizeof(c[sz]));
            // val[sz]=0;
            c[u][s[i]]=sz++;
        }
        u=c[u][ch];
        cnt[u]++;
    }
    // val[u]=v;
}
int query(char s[])
{
    int u=0,n=strlen(s);
    for (int i=0; i<n; i++)
    {
        char ch = idx(s[i]);
        if (!c[u][ch]||u!=0&&cnt[u]<=1)
            return i;   //查询最大匹配长度
        u=c[u][ch];
    }
    return n;   //查询最大匹配长度
    // return cnt[u];  /*查询该字符串出现的次数*/
}

```