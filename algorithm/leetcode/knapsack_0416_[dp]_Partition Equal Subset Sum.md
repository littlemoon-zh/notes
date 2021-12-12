
转化为01背包问题。


```python
def canPartition(self, nums: List[int]) -> bool:
    s = sum(nums)
    if s & 1: return False
    s = s // 2

    dp = [False] * (s + 1)
    dp[0] = True

    for i in range(len(nums)):
        for v in range(s, nums[i]-1, -1):
            dp[v] = dp[v] or dp[v - nums[i]]

    return dp[-1]
```
