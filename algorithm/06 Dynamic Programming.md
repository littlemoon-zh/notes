# Dynamic Programming


## From memo to dp

- 使用递归解，不依赖外部变量，返回值是答案，函数参数就是状态
- 加入备忘，求解同一组参数时直接返回
- 转换为bottom-up的dp


## 背包问题

[416. Partition Equal Subset Sum](https://leetcode.com/problems/partition-equal-subset-sum/)

问题转换，就是看是否可以从数组中取一些数，它们的和是`sum/2`，这就变成了01背包问题。

优化：一维数组优化，从后往前。

参考：背包问题九讲。

```
ZeroOne(cost, weight):
  for v = V..cost:
    f[v] = max(f[v], f[v-cost] + weight)

for i in n:
  ZeroOne(cost[i], weight[i])
```


## 区间dp

```
dp[i,j] = f(dp[i,k], dp[k+1,j]) + cost
```

这种转移方程类似于分治，把[i, j]这个大问题分解成[i, k]和[k+1, j]两个小问题，然后在所有这些分解方法中，挑选一个最优的。也就是组合优化问题。


[471. Encode String with Shortest Length](https://leetcode.com/problems/encode-string-with-shortest-length/) 和它的逆问题 [394. Decode String](https://leetcode.com/problems/decode-string/)





## 二维dp

[62. Unique Paths](https://leetcode.com/problems/unique-paths/)

## 树dp


暂时不太理解的题：

[300. Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/)

dp好做，二分不太好想。

