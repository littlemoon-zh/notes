

## Built-in functions

- `divmod(a, b)`
- `bin`, `bin(3) = '0b11'`
- `ord` and `chr`, `ord('a') = 95`, `chr(95) = 'a'`
- `round()` round to nearest int
- `min` `max` `sum`
- `int('1010', base=2)`, `int('1010', 2)`, `36 >= base >= 2`

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


## 奇技淫巧

- 求Transpose:
```python
A = [[1,2], [3,4]]

tA = zip(*A)
# tA = [[1,3], [2,4]]
```

- 利用`～`前后遍历
```python
for j in range(n/2):
  row[j], row[~j] = row[~j], row[j]
'''
0, ~0 = -1
1, ~1 = -2
...
'''
```
