
对每个房子，找到距离这个房子最近的加热器的位置（使用二分来找）。在这些位置中取最大值，以确保满足所有的情况。

对于一个非降序数组，bisect_left位置时第一个大于等于target的位置，所以该位置和前一个位置（如果存在的话），就是距离target最小的两个可能点，在这两个选一个就行了。

```python
import bisect

class Solution:
    def findRadius(self, houses: List[int], heaters: List[int]) -> int:
        heaters.sort()
        res = 0
        for h in houses:
            x = bisect.bisect_left(heaters, h)
            cur = float('inf')
            if x < len(heaters): cur = abs(heaters[x] - h)
            if x > 0: cur = min(cur, abs(heaters[x-1] - h))
            res = max(res, cur)
        return res
```
