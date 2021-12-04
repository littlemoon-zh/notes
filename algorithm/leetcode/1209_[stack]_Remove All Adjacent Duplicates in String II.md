其实思路很简单，就是使用一个stack，当发现栈里面最后有重复的大于等于k个元素时，就删除k个。

问题在于如何高效判断以及高效删除，如果使用最简单的方法，每次判断是往前遍历，删除也是再遍历一下，所以最坏时间复杂度就是 `O(n^2)`。

我的这种解法，把前面相同的合并到一个stack item里面，从而实现了查找和删除都是`O(1)`,所以总的时间复杂度是`O(n)`.

```python
def removeDuplicates(self, s: str, k: int) -> str:
    stack = [[s[0], 1]]

    for i in range(1, len(s)):
        if stack and stack[-1][0] == s[i]:
            ch, times = stack.pop(-1)
            stack.append([s[i], times + 1])
        else:
            stack.append([s[i], 1])
        while stack and stack[-1][1] >= k:
            stack.pop()

    return ''.join([x[0] * x[1] for x in stack])
```
