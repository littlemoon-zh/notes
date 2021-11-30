

```python
class Solution:
  def maxSumTwoNoOverlap(self, nums: List[int], a: int, b: int) -> int:
    s = [0] * (len(nums) + 1)
    # prefix sum
    for i in range(1, len(s)):
      s[i] = s[i-1] + nums[i-1]
    def work(a, b):
      res = 0
      maxa = 0
      for i in range(a + b, len(nums) + 1):
        maxa = max(maxa, s[i-b] - s[i-a-b])
        res = max(res, maxa + s[i] - s[i-b])
      return res
    
    return max(work(a, b), work(b, a))
```
