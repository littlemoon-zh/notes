
和84题一样的写法，只不过在84中，柱状图的高度时给定的，而且不再变化，在这里需要自己计算，而且在每一行柱状图的高度都有些不同。



```python
class Solution:
    def maximalRectangle(self, M: List[List[str]]) -> int:
        try:
            m, n = len(M), len(M[0])
        except:
            return 0
        hs = [0] * n
        s = 0
        res = 0
        for i in range(n):
            if M[0][i] == "1":
                s += 1
                res = max(res, s)
                hs[i] = 1
            else:
                s = 0

        for i in range(1, m):
            for j in range(n):
                if M[i][j] == '1': hs[j] = hs[j] + 1
                else: hs[j] = 0
            
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

            for i in range(len(hs)):
                res = max(res, hs[i] * (right[i] - left[i] - 1))
                
        return res
```
