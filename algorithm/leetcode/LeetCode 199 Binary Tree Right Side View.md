# 199 Binary Tree Right Side View
[链接](https://leetcode.com/problems/binary-tree-right-side-view/)
![image](https://user-images.githubusercontent.com/85326814/137026122-417bdd1e-b901-4be4-b786-7de41bc3d388.png)

## 解法1

这题比较容易想到的思路是层序遍历，只要修改一下顺序就行了，因为一般而言我们的层序遍历入队列是先入left，后入right；但是这里为了在队头找到答案，我们就修改一下入队列的方式，不过不修改也是没问题的，因为知道每一层的长度，直接把每层最后一个加到res中就可以了。

```java
class Solution {
    public List<Integer> rightSideView(TreeNode root) {
        List<Integer> res = new LinkedList<>();
        if (root == null) return res;
        
        Deque<TreeNode> q = new LinkedList<>();
        q.offer(root);

        while (!q.isEmpty()) {
            int size = q.size();
            for (int i = 0; i < size; i++) {
                TreeNode cur = q.poll();
                if (i == 0) res.add(cur.val); // the right most node
                if (cur.right != null) q.offer(cur.right); // offer right first
                if (cur.left != null) q.offer(cur.left);
            }
        }
        return res;
    }
}
```

## 解法2

实际上，我们发现，右视图是什么意思呢？就是在同一个level里面，我们优先输出靠右的节点。比如我们每一层要输出一个节点，我们在遍历的时候，采用传统的先序遍历，在某一个level遇到一个节点就把它当做这一层的输出节点，这样一来由于我们后遍历right，就会导致right的节点覆盖之前的节点，也就得到了我们的正确答案。

我们可以使用一个map来存储level和value的映射情况，最终输出时，按照level有序输出value就行，或者为了省事直接使用TreeMap来存储，这样保证输出时，一定是按照level顺序，而不需要自己额外排序了。

上述思路可以变成如下的代码：

```java
class Solution {
    TreeMap<Integer, Integer> map = new TreeMap<>();

    public void dfs(TreeNode root, int level) {
        if (root == null)
            return;
        
        map.put(level, root.val);
        dfs(root.left, level + 1);
        dfs(root.right, level + 1);
    }
    public List<Integer> rightSideView(TreeNode root) {
        dfs(root, 0);
        return new ArrayList<>(map.values());
    }
}
```

## 解法3

看起来解法2似乎有些复杂了，好像不用TreeMap这种复杂的结构。我们再次观察题目，每当要添加一个节点到res的时候，该节点一定是这一层中的最右节点。如果我们修改一下遍历顺序，优先遍历右子树，比如right, left, root,这样一来我们可以保证一定会先考虑右边。此外，我们在当前的level比res的size大的时候才添加，可以保证忽略那些不应该被添加的节点。

```java
class Solution {
    List<Integer> res = new LinkedList<>();

    public void dfs(TreeNode root, int level) {
        if (root == null)
            return;
        if (level >= res.size()) res.add(root.val); // key point
        dfs(root.right, level + 1);
        dfs(root.left, level + 1);
    }
    public List<Integer> rightSideView(TreeNode root) {
        dfs(root, 0);
        return res;
    }
}
```


