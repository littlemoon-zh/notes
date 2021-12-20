
![image](https://user-images.githubusercontent.com/85326814/146698406-76d019e2-937a-4dae-b53c-39cbc31c3ce3.png)


```python
class NumArray:

    def __init__(self, nums: List[int]):
        self.nums = [0] * len(nums)
        self.bit = [0] * (len(nums) + 1)
        
        for i, num in enumerate(nums):
            self.update(i, num)
        
    def update(self, index: int, val: int) -> None:
        delta = val - self.nums[index]
        self.nums[index] = val
        
        i = index + 1
        while i < len(self.bit):
            self.bit[i] += delta
            i += i & -i
    
    def get(self, i):
        i += 1
        res = 0
        while i:
            res += self.bit[i]
            i -= i & -i
        return res
    
    def sumRange(self, left: int, right: int) -> int:
        return self.get(right) - self.get(left-1)
```
