# 数位DP

## 题目描述
给定两个10^18范围内的数字`l, r`，问在`[l, r]`区间内，有多少单调的数字，单调的数字的定义是：高位恒小于等于低位或低位恒小于等于高位。

## 解题思路
通过数位DP的方式，求出来所有**高位恒小于等于低位的数**的个数以及**低位恒小于等于高位的数**的个数，再减去**所有位相等的数**的个数

## Tips
* 数位DP是通过记忆化搜索的方式，也就是说，求出来的位对所有数进行数位DP都适用，不需要每次都`memset`，可以大幅度降低时间复杂度。
* 主要难点在于对状态的判断

## 代码

```C++
//BNU52325
int num[20],pos;
ll dp1[20][10];//20位表示数的长度 10表示前一位的值
ll dp2[20][10][2];//20位表示数的长度 10表示前一位的值 2表示是否是第一位
ll dfs1(int pos, int pre, bool limit){//统计单调递增 pos表示当前位的下标 pre表示前一位的状态 limit表示是否受数字上限大小约束
	if (pos < 1) return 1;
	if (!limit && dp1[pos][pre]!=-1)  return dp1[pos][pre];
	int mx = limit?num[pos]:9;
	ll ret = 0;
	for (int i = pre; i <= mx ; i++) //保证当前位比前一位大
		ret += dfs1(pos-1, i, limit && i == num[pos]);
	if (!limit)  dp1[pos][pre] = ret; //仅存储无约束条件的数 保证记忆化搜索的正确性
	return ret;
}
ll dfs2(int pos, int pre, bool limit, bool first){//单调递减 pos表示当前位的下标 pre表示前一位的状态 limit表示是否受数字上限大小约束 first表示是否是最高位
	if (pos < 1) return 1;
	if (!limit && dp2[pos][pre][first]!=-1)  return dp2[pos][pre][first];
	int mx;
	if (first && limit)  mx = num[pos];
	else if (first && !limit)  mx = 9;
	else if (!first && limit)  mx = min(num[pos], pre);
	else  mx = pre;
	ll ret = 0;
	for (int i = 0; i <= mx ; i++) //保证当前位比前一位小
		ret += dfs2(pos-1, i, limit && i == num[pos], first&&i==0);
	if (!limit)  dp2[pos][pre][first] = ret; //仅存储无约束条件的数 保证记忆化搜索的正确性
	return ret;
}
ll solve(ll x){
	ll tmp = x;
	pos = 0;
	while (x) {
		num[++pos] = x % 10;
		x /= 10;
	}
	ll ret = 0;
	ret += dfs1(pos, 0, true) - 1; //去除0
	ret += dfs2(pos, 9, true, true) - 1; //去除0
	ll bas = 1;
	while (tmp >= bas) { //去除所有位相等的情况 如111, 33333, 55, 6666
		ret -= min(9LL, tmp/bas);
		bas = bas * 10 + 1;
	}
	return ret;
}
int main(){
	//初始化记忆标记
	memset(dp1, -1, sizeof(dp1));
	memset(dp2, -1, sizeof(dp2));
	
	int n;
	scanf("%d",&n);
	while (n--) {
		ll l,r;
		scanf("%lld%lld", &l, &r);
		printf("%lld\n",solve(r) - solve(l-1));
	}
	return 0;
}
```