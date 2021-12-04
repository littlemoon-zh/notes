

单调栈解法：

```python
def trap(self, hs: List[int]) -> int:
    res, s = 0, []
    for i, h in enumerate(hs):
        while s and h > hs[s[-1]]:
            j = s.pop(-1)
            if s: res += (i - s[-1] - 1) * (min(h, hs[s[-1]]) - hs[j])
        s.append(i)

    return res
```
