# 二分图

二分图主要是说，一个点集A和一个点集B，A与B无公共元素，且各点集内部点无连线
 
## 概念最大独立集：求一个二分图中最大的一个点集，该点集内的点互不相连。

最小顶点覆盖数：在二分图中，用最少的点，让所有的边至少和一个点有关联。换句话说，假如选了一个点就相当于覆盖了以它为端点的所有边，你需要选择最少的点来覆盖所有的边。

最小路径覆盖：找出最小的路径条数，使这些路径覆盖图中所有点。

## 计算方法
最大独立集 = 顶点数 - 最大匹配数 = vN + uN - hungary()

最小顶点覆盖数 = 最大匹配数 = hungary()

最小路径覆盖＝｜G｜－最大匹配数