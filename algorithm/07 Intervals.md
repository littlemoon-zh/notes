# Intervals

区间问题策略
- 常用的预处理方式是排序，比如根据左端点排序或者右端点排序；
- 还会涉及到集合的Union Intersection Difference等操作；
- 要关注区间的开闭；
- 画图最好理解；
- 贪心策略在区间题中还挺常用的


## [252. Meeting Rooms](https://leetcode.com/problems/meeting-rooms/)

思路：sorting + intersection判断

## [253. Meeting Rooms II](https://leetcode.com/problems/meeting-rooms-ii/)

思路：sorting + heap + 计算同时有几个区间intersection

```python
from heapq import *

class Solution:
    def minMeetingRooms(self, intervals: List[List[int]]) -> int:
        intervals.sort()
        q = []
        res = 0
        for s, e in intervals:
            while q and q[0] < s:
                heappop(q)
            res = max(res, 1 + len(q))
            heappush(q, e-1)
        return res
```

## [763. Partition Labels](https://leetcode.com/problems/partition-labels/)

思路：hash + intersection，这题可以看成区间问题，也可以看成是贪心的策略，或者双指针；

## [56. Merge Intervals](https://leetcode.com/problems/merge-intervals/)

思路：sorting + Union

## [57. Insert Interval](https://leetcode.com/problems/insert-interval/)

问题转化：step1 有序序列插入问题，可以使用二分确定插入位置；step2 转换成merge intervals问题；

## [495. Teemo Attacking](https://leetcode.com/problems/teemo-attacking/)

思路：扩展区间的问题

```python
class Solution:
    def findPoisonedDuration(self, timeSeries: List[int], duration: int) -> int:
        right = -1
        res = 0
        for start in timeSeries:
            # 如果在上一次攻击范围内 看看是否能增加新的攻击范围
            if start <= right:
                res += max(start + duration - 1 - right, 0)
                right = max(right, start + duration - 1)
            # 如果已经过了上次的攻击范围 直接+duration即可
            else:
                res += duration
                right = start + duration - 1
        return res
```

## [435. Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/)

If we sort by the start, then:
```
[1, 100], [2,3], [3,4], [4,5], [10,20], [20, 24]
```

In this case, it's obviously wrong! Sort by the end so that current interval has no impact on intervals behind.

## [986. Interval List Intersections](https://leetcode.com/problems/interval-list-intersections/)

总体思路类似于merger two sorted array，用两个指针。


