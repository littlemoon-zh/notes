# Dynamic Programming


## 一维dp

### 矩阵链乘类 

```
dp[i,j] = f(dp[i,k], dp[k+1,j]) + g()
```

这种转移方程类似于分治，把[i, j]这个大问题分解成[i, k]和[k+1, j]两个小问题，然后在所有这些分解方法中，挑选一个最优的。也就是组合优化问题。


[471. Encode String with Shortest Length](https://leetcode.com/problems/encode-string-with-shortest-length/) 和它的逆问题 [394. Decode String](https://leetcode.com/problems/decode-string/)


## 二维dp

[62. Unique Paths](https://leetcode.com/problems/unique-paths/)

## 树dp

