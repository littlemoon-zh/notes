

## Built-in functions

- `divmod(a, b)`
- `bin`, `bin(3) = '0b11'`
- `ord` and `chr`, `ord('a') = 95`, `chr(95) = 'a'`
- `round()` round to nearest int
- `min` `max` `sum`

## math module

```python
import math
```
- `math.factorial(x)`

## bisect module

- 常用于二分 针对有序数组

```python
import bisect

# 返回第一个大于等于x的位置
bisect.bisect_left(arr, x, lo, hi)

# 返回第一个大于x的位置 也就是有序数组中的插入位置
bisect.bisect_right(arr, x, lo, hi)
```
