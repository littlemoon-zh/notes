和最大上升子串类似。`dp[i][j]`表示以`i`结尾，公差为`j`的等差数列的长度。由于公差范围很大，所以用hash来存储。


```python
class Solution:
    def longestArithSeqLength(self, nums: List[int]) -> int:
      dp = [{} for i in range(len(nums))]
      res = 2
      for i in range(len(nums)):
        for j in range(i):
          d = nums[j] - nums[i]
          dp[i][d] = 2
          if d in dp[j]:
            dp[i][d] = max(dp[i][d], dp[j][d] + 1)
            res = max(res, dp[i][d])          
      return res
```
