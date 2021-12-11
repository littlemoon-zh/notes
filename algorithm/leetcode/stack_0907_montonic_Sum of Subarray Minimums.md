

对于一个数字，它会被算几遍呢？假设它右边严格大于的有right个，左边大于等于它的有left个，那么这个数在结果中一共出现left * right次。

如何计算每个点的left和right呢？使用单调递增的栈。

单调递增的栈有一个性质，当i位置的数让之前栈中的j元素出栈的话，那么在`[j, i)`区间上，j一定是最小的。

![image](https://user-images.githubusercontent.com/85326814/145665754-60173583-0b99-4694-8746-5ee2fc29e776.png)

**这题另一个tricky的地方在于重复性，为什么两次计算中一次严格大于，一次是大于等于呢？目的就是重复元素只算一次。**

```python
def sumSubarrayMins(self, arr: List[int]) -> int:
    n = len(arr)
    left = [1] * n
    right = [1] * n

    s = []

    for i, num in enumerate(arr):
        while s and num < arr[s[-1]]:
            j = s.pop()
            right[j] = i - j
        s.append(i)
    while s:
        j  = s.pop()
        right[j] = n - j

    s = []
    for i in range(n-1, -1, -1):
        while s and arr[i] <= arr[s[-1]]:
            j = s.pop()
            left[j] = j - i
        s.append(i)
    while s:
        j = s.pop()
        left[j] = j + 1

    res = 0
    for i in range(n):
        res += arr[i] * left[i] * right[i]
    return int(res % (1e9 + 7))
```
