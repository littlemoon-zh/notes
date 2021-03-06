# Permutation problems

Permutation是一个基础并且重要的搜索、回溯类问题，也是我们理解递归、搜索、回溯的一个好的突破口，因为它简单且变化很多。

## 计算Permutation的几种方法

### 1 使用used数组

基础的写法，就是dfs+backtracking。

略。

### 2 使用NextPermutation方法
```python
res = []
for i in range(math.factorial(len(nums))):
  res.append(nextPernutation(nums))
```

其中nextPermutation写法就是迭代法找到next。见 [LeetCode 31 Next Permutation](https://leetcode.com/problems/next-permutation/)
### 3 另一种递归形式
```python
def permute(nums):
    res = []
    def dfs(pos, p, nums):
        if not nums:
            res.append(p[:])
            return
        for i in range(len(nums)):
            dfs(pos + 1, p + [nums[i]], nums[:i] + nums[i+1:])
    dfs(0, [], nums)
    return res
```

有些题目有去重的需求，实质上就是说在同一个递归深度，使用的元素不能和上次相同，为了实现这一点，我们需要对原始数组排序，让相同的元素在一起，然后每次记录一个last变量，如果发现当前使用的元素和上次相同，直接continue就可以了。
