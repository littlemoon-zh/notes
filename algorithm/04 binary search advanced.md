## Advanced Binary Search

所谓的高级是指，很难看出来用二分来做。


[2080. Range Frequency Queries](https://leetcode.com/problems/range-frequency-queries/)

也是逆向思维的一个运用，反着来。

```python
class RangeFreqQuery:
  def __init__(self, arr: List[int]):
    self.num_to_pos = defaultdict(list)
    for i, num in enumerate(arr):
      self.num_to_pos[num].append(i)

  def query(self, left: int, right: int, value: int) -> int:
    if value not in self.num_to_pos: return 0
    arr = self.num_to_pos[value]
    le = bisect.bisect_left(arr, left)
    ri = bisect.bisect_right(arr, right)
    return ri - le
                
```

