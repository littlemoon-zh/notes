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



## 双指针最值问题

总体思路就是，在可行范围内滑动右指针，然后不断收缩左指针。

[Number of Operations to Decrement Target to Zero](https://binarysearch.com/problems/Number-of-Operations-to-Decrement-Target-to-Zero)

转换问题，最大和最小是可以转化的。两边和中间是可以转化的。这题非常好，把两边的最小转换成中间的最大。

```python
class Solution:
    def solve(self, nums, target):
        if target == 0: return 0
        target = sum(nums) - target
        lo, hi = 0, 0
        cur = 0
        res = -1
        if target == 0: return len(nums)
        if target < 0: return -1
        while hi < len(nums):
            cur += nums[hi]
            if cur >= target:
                while cur >= target and lo <= hi:
                    if cur == target:
                        res = max(res, hi - lo + 1)
                    cur -= nums[lo]
                    lo += 1
            hi += 1
        if res == -1: return -1
        return len(nums) - res
```

## sliding window

[209. Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/)


## greedy

[11. Container With Most Water](https://leetcode.com/problems/container-with-most-water/)

