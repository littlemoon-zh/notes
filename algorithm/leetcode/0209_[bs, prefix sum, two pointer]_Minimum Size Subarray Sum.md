
## two pointer 

```python
class Solution:
  def minSubArrayLen(self, target: int, nums: List[int]) -> int:
    lo, hi = 0, 0
    res = float('inf')
    s = 0
    while hi < len(nums):
      s += nums[hi]
      while s >= target and lo <= hi:
        res = min(res, hi - lo + 1)
        s -= nums[lo]
        lo += 1
      hi += 1
    return res if res != float('inf') else 0
```



## prefix + binary search

```python
import bisect

class Solution:
  def minSubArrayLen(self, target: int, nums: List[int]) -> int:
    s = [0] * (len(nums) + 1)
    for i in range(1, len(s)): 
      s[i] = s[i-1] + nums[i-1]
    res = float('inf')
    for i in range(1, len(s)):
      if s[i] - target >= 0:
        t = bisect.bisect_right(s, s[i]-target, 0, i)
        if t > 0: res = min(res, i - t + 1)
    return res if res != float('inf') else 0
```
