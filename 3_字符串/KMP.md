# KMP

## 说明
时间复杂度：O(n+m)

* `char T[]`：被匹配串
* `char P[]`：子串
* `char f[]`：存储失败指针，大小需要大于等于与`P[]`
* 返回值：所有匹配上的点的下标

`f[i]`表示：**如果子串**`P[i]`**匹配原串**`T[j]`**失败，则使用**`P[f[i]]`**匹配**`T[j]`

## 使用方法
1. `scanf("%s %s",T,P);` 输入被匹配串T，匹配子串P
2. `vector<int>ans = KMP(T, P, f);` 需要自行开辟失败指针数组`f[]`，返回每一个匹配位置下标


## Tips
* `getFail()`会在`KMP`中自动调用
* 字符串均从`s[0]`开始
* `KMP`函数中的`for`的`while`利用了字符串最后一个字符是`\0`的特性，当匹配的是int数组时需要特别注意（坑题示例[HDU 5918 Sequence I](http://acm.hdu.edu.cn/showproblem.php?pid=5918)）

## 模版
```C++
void getFail(char *P, int *f){
    int lenP = (int)strlen(P);
    f[0] = 0; f[1] = 0; //递推边界初值
    for (int i=1; i<lenP; i++) {
        int j = f[i];
        while (j&&P[i]!=P[j]) {
            j = f[j];
        }
        f[i+1] = (P[i]==P[j]?j+1:0);
    }
}
vector<int> KMP(char *T, char *P, int *f){
    int lenT = strlen(T);
    int lenP = strlen(P);
    getFail(P, f);
    int j = 0;  //当前结点的编号，初始为0号始点
    vector<int> ans;
    for (int i = 0; i<lenT; i++) { //文本串当前指针
        while (j && P[j]!=T[i])    //顺着失败边走，直到可以匹配
            j = f[j];
        if (P[j] == T[i])
            j++;
        if (j==lenP)
            ans.push_back(i-lenP+1);
    }
    return ans;
}
```