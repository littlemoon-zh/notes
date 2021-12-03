
需要深刻理解递归的运行机制。

和[341. Flatten Nested List Iterator](https://leetcode.com/problems/flatten-nested-list-iterator/) 略有类似。需要思考如何iterator。

```python
class BSTIterator:

    def __init__(self, root: Optional[TreeNode]):
        self.s = []
        while root:
            self.s.append(root)
            root = root.left

    def next(self) -> int:
        res = self.s.pop(-1)
        if res.right:
            it = res.right
            while it:
                self.s.append(it)
                it = it.left
        
        return res.val
        
    def hasNext(self) -> bool:
        return self.s
```
