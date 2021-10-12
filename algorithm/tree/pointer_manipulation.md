# 树的指针操作

感觉这类题目是树里面最难的，尤其是要求in-place操作时，很多操作无法想到。

## 114 Flatten Binary Tree to Linked List

![image](https://user-images.githubusercontent.com/85326814/136897568-19b71ac1-53c0-4708-a475-346ee6c737e1.png)

要求按照先序的方式修改指针；先序遍历和后序遍历实际上有一种对称关系，先序是`root, left, right`, 后序是`left, right, root`，那么反后序是`right, left, root`，这和先序刚好是对称的。

我们看这个图，比较容易想到用反后序来做。

```python
class Solution:
    def flatten(self, root: Optional[TreeNode]) -> None:
        """
        Do not return anything, modify root in-place instead.
        """
        self.prev = None
        def dfs(root):
            if not root: return
            
            dfs(root.right)
            dfs(root.left)

            root.right = self.prev
            root.left = None
            self.prev = root
        dfs(root)
```

## 426 Convert Binary Search Tree to Sorted Doubly Linked List


```java
class Solution {
    public Node treeToDoublyList(Node root) {
        if (root == null) {
            return null;
        }
        
        Node leftHead = treeToDoublyList(root.left);
        Node rightHead = treeToDoublyList(root.right);
        root.left = root;
        root.right = root;
        return connect(connect(leftHead, root), rightHead);
    }
    
    // Used to connect two circular doubly linked lists. n1 is the head of circular DLL as well as n2.
    private Node connect(Node n1, Node n2) {
        if (n1 == null) {
            return n2;
        }
        if (n2 == null) {
            return n1;
        }
        
        Node tail1 = n1.left;
        Node tail2 = n2.left;
        
        tail1.right = n2;
        n2.left = tail1;
        tail2.right = n1;
        n1.left = tail2;
        
        return n1;
    }
}
```
