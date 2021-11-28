博弈论，偶数必胜，奇数必败，用数学归纳法证明。

```python
class Solution:
  def divisorGame(self, n: int) -> bool:
    return n % 2 == 0
```
