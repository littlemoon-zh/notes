
最大的difference一定出现在两端。

```python
class Solution:
    def maxAncestorDiff(self, root: Optional[TreeNode]) -> int:
      self.res = float('-inf')
      def dfs(root):
        if not root: return [float('inf'), float('-inf')]
        lmin, lmax = dfs(root.left)
        rmin, rmax = dfs(root.right)
        l = min(lmin, rmin, root.val)
        r = max(lmax, rmax, root.val)
        self.res = max(self.res, abs(root.val - l), abs(root.val - r))
        return [l, r]
      
      dfs(root)
      return self.res
```
