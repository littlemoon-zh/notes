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

[494. Target Sum](https://leetcode.com/problems/target-sum/)

这题也是个背包问题，只不过比上面的更难发现，更难想到变形：

```
positive + negative = target
positive - negative = total
-->
postive = (target + total) / 2
```

不过直接递归做，使用备忘录也是可以的，很好想，转换成类似在二叉树进行搜索，每步有两个选择，+当前或者-当前，或许从这里也可以看出是背包问题，因为有两种选择，所以很类似于01背包。

```python
def findTargetSumWays(self, nums: List[int], target: int) -> int:
    memo = {} 
    def dfs(i, cur):
        if (i, cur) in memo: return memo[i, cur]
        if i == len(nums): return 1 if cur == target else 0

        res = dfs(i + 1, cur + nums[i]) + dfs(i + 1, cur - nums[i])
        memo[i, cur] = res
        return res
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

