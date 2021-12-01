

我们维护一个变量s3，表示a[k]这个值是从后往前的一个‘最大值’，之所以要加引号，是因为这个最大值并不是真正的最大值，而是必须在它的左边有个值比它大。

**注意这里也不是次大值，比如这种情况。[1,7,8]从后往前到7的时候，次大值是7，但这并不是s3，s3要满足它的左边有一个值比它大。**

这样以来，当我遇到一个数num比s3小，也说明了，还有一个数介于num和s3之间，且是最大的。

找这样一个s3的过程，使用单调栈。我们维护一个递减栈，所以我们每次出栈的时候，那个数字一定满足左边有个数比它大，`while s and num > s[-1]:`,所以这就是目标s3。

很难解释。。。

代码简单，思想精巧。

```pythhon
class Solution:
    def find132pattern(self, nums: List[int]) -> bool:
        s3 = float('-inf')
        s = []
        for num in reversed(nums):
            if num < s3: return True
            while s and num > s[-1]:
                s3 = max(s3, s.pop(-1))
            s.append(num)
        return False
```
