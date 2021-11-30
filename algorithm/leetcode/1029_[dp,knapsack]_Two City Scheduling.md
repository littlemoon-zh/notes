

限制性选择问题，背包问题，从2n个里面选出n个，且满足值最小。

memo + dfs

```python
class Solution:
  def twoCitySchedCost(self, costs: List[List[int]]) -> int:
    n = len(costs) // 2
    memo = {}
    def dfs(i, n):
      if (i, n) in memo:
        return memo[i, n]
      if i == len(costs):
        if n == 0:
          return 0
        return float('inf')
      
      a, b = costs[i]
      select_a = a + dfs(i + 1, n - 1)
      not_select = b + dfs(i + 1, n)
      res = min(select_a, not_select)
      memo[i, n] = res
      return res
    return dfs(0, n)
```

贪心比较难想。。
