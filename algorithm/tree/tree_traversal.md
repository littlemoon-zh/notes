# 树的遍历

感觉几乎所有树的问题都可以归结为遍历问题，有时候看起来很复杂的问题都可以通过稍稍修改遍历方式以及遍历时的处理方式而简单的完成。

一般而言树的遍历有四种方式：

1. pre-order, root, left, right
2. in-order, left, root, right
3. post-order, left, right, root 和pre-order有一定的对称性
4. level-order, first level, second level...

上述方式都可以通过递归以及迭代的方法实现。在实际应用中应该灵活选取实现方式。

此外，这些遍历在实际使用时也要灵活运用，比如有时候right, left, root这种反后序更加好用。




## 相关LeetCode题解


- [199 Binary Tree Right Side View](../leetcode/LeetCode%20199%20Binary%20Tree%20Right%20Side%20View.md)



