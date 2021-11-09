# Two pointers


## slow and fast pointers

[26. Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/) & [80. Remove Duplicates from Sorted Array II](https://leetcode.com/problems/remove-duplicates-from-sorted-array-ii/)

可以推广到k，即每个元素最多出现k次

```python
def removeDuplicates(self, nums: List[int]) -> int:
    slow, fast = 0, 0
    for fast in range(len(nums)):
        if slow >= k and nums[fast] == nums[slow-k]:
            continue
        nums[slow] = nums[fast]
        slow += 1
    return slow
```

[287. Find the Duplicate Number](https://leetcode.com/problems/find-the-duplicate-number/)



## sliding window

[209. Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/)
