
```python
class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        q, res = [], []
        for i, num in enumerate(nums):
            while q and nums[q[-1]] <= num:
                q.pop(-1)
            q.append(i)
            if i - q[0] + 1 > k: q.pop(0)

            if i >= k - 1: res.append(nums[q[0]])
        return res
```
