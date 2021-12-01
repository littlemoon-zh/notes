
维护一个last不断迭代即可，不要忘了最后一个要加上。

```python
class Solution:
    def merge(self, ts: List[List[int]]) -> List[List[int]]:
        ts.sort()
        
        last = ts[0][0]
        last_int = ts[0]
        i = 0
        res = []
        while i < len(ts):
            if ts[i][0] > last:
                res.append(last_int)
                last = ts[i][1]
                last_int = ts[i]
            else:
                last_int[1] = max(last_int[1], ts[i][1])
                last = last_int[1]
            i += 1
        res.append(last_int)
        return res
        
```
