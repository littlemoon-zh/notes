
这个解法太妙了。


[2111. Minimum Operations to Make the Array K-Increasing](https://leetcode.com/problems/minimum-operations-to-make-the-array-k-increasing/)，实际上就是这题的一个伪装。

`O(nlogn)` solution:

```python
def lengthOfLIS(self, nums: List[int]) -> int:
    lis = []
    for num in nums:
        if not lis or num > lis[-1]:
            lis.append(num)
        else:
            idx = bisect.bisect_left(lis, num)
            lis[idx] = num

    return len(lis)
```
