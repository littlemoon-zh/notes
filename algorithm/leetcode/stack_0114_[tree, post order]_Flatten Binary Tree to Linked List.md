

逆后续遍历

![image](https://user-images.githubusercontent.com/85326814/144722745-df2deed5-0442-4531-9cfa-3459668e03b3.png)


```python
def flatten(self, root: Optional[TreeNode]) -> None:
    """
    Do not return anything, modify root in-place instead.
    """
    self.last = None

    def dfs(root):
        if not root: return 
        dfs(root.right)
        dfs(root.left)
        root.right = self.last
        root.left = None
        self.last = root

    dfs(root)
```
