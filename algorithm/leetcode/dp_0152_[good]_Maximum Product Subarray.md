
因为有负数的存在，需要同时考虑前面的最小值和最大值，因为在遇到负数时，之前的最小值可能变成一个最大值。

```python
def maxProduct(self, nums: List[int]) -> int:
    premin, premax = nums[0], nums[0]
    res = premax
    for i in range(1, len(nums)):
        if nums[i] < 0:
            premin, premax = premax, premin

        premin = min(nums[i], nums[i] * premin)
        premax = max(nums[i], nums[i] * premax)

        res = max(res, premax)

    return res
```
