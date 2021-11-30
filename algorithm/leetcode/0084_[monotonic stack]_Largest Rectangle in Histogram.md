
思路很简单，就是对每一个位置，看它能向左向右最大扩张到哪里。如果用暴力来做，就是`n^2`，但是使用单调队列，用空间换时间，进行一定**pre-computation**将时间复杂度变成线性的。

```python
class Solution:
  def largestRectangleArea(self, hs: List[int]) -> int:
    left, right = [-1] * len(hs), [len(hs)] * len(hs)
    s = []
    for i, h in enumerate(hs):
      while s and h < hs[s[-1]]:
        idx = s.pop(-1)
        right[idx] = i
      s.append(i)
    s = []
    for i, h in reversed(list(enumerate(hs))):
      while s and h < hs[s[-1]]:
        idx = s.pop(-1)
        left[idx] = i
      s.append(i)

      res = 0
      for i in range(len(hs)):
        res = max(res, hs[i] * (right[i] - left[i] - 1))
      return res
```
