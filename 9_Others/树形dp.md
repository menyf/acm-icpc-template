<!-- toc -->
# 树形DP
树形 DP 就是在树上做的 DP，大部分都是用子节点的信息推导出父节点的信息。在考虑问题可以被抽象出一个树状的模型的时候，可以尝试考虑一下树形DP。

## 树的表示
在树形 dp 中的树的形状各异，所以我习惯用`vector`来存储一个节点的所有子节点。用`push_back`方法去加入一个子节点，这将比自己实现的左子树右孩子的写法更安全可靠。（作为代价，可能会牺牲一点点的效率。）

## 例题（[POJ 2342 Anniversary party](http://poj.org/problem?id=2342)）
很简单的一个树形 dp 的问题，可以很容易的考虑到每个人都有两种选择，也就是来或者不来。那么接下来的部分就容易很多了，我们可以很容易的得到状态表示，$$dp[i][0]$$ 表示第 $$i$$ 个人不来；$$dp[i][1]$$ 表示第 $$i$$ 个人来。

在状态的转移方面，可以得到，如果第 $$i$$ 个人不来的话，那么他的所有子节点就都有来或不来两种状态，也就是 $$dp[i][0] = max_{\textbf{j is son of i}}\{dp[j][0], dp[j][1]\}$$；同时如果第 $$i$$ 个人来的话，那么他的所有子节点都只能不来，也就是是 $$dp[i][1] = max_{\textbf{j is son of i}}\{dp[j][0]\}$$。

在得到了状态转移和表示之后，代码就很容易得到了。我们在写树形 dp 的时候，一般会写成类似于搜索的样子，只不过会在搜索中加入备忘录，这种写法也叫作***记忆化搜索***。

代码中还有关于把无根树转换成有根树的方法，任意选择一个节点作为根节点，从这个根节点开始 DFS，要注意因为在建边的时候都是双向边，所以在DFS的过程中，一定要注意判断当前节点是否是我们设置的父节点，防止循环的递归或者可能产生错误的答案。

```c++
#include <cstdio>
#include <cstring>
#include <algorithm>
#include <vector>
using namespace std;
const int maxn = 6010;

int dp[maxn][2];
vector<int> g[maxn];
int w[maxn];

int get_dp(int x, int s, int fa) {
    if (dp[x][s] != -1) {
        return dp[x][s];
    }
    dp[x][s] = 0;
    if (s) {
        dp[x][s] = w[x];
        for (size_t i = 0; i < g[x].size(); i++) {
            if (g[x][i] != fa) {
                dp[x][s] += get_dp(g[x][i], 0, x);
            }
        }
    }
    else {
        for (size_t i = 0; i < g[x].size(); i++) {
            if (g[x][i] != fa) {
                dp[x][s] += max(get_dp(g[x][i], 0, x), get_dp(g[x][i], 1, x));
            }
        }
    }
    return dp[x][s];
}


int main() {
    int n;
    while (scanf("%d", &n) != EOF) {
        memset(dp, -1, sizeof dp);
        for (int i = 1; i <= n; i++) {
            g[i].clear();
        }

        for (int i = 1; i <= n; i++) {
            scanf("%d", &w[i]);
        }
        int u, v;
        while (scanf("%d%d", &u, &v), u||v) {
            g[u].push_back(v);
            g[v].push_back(u);
        }

        printf("%d\n", max(get_dp(1, 0, -1), get_dp(1, 1, -1)));
    }
    return 0;
}

```