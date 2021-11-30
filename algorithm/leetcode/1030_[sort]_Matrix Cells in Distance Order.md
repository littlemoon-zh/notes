You are given four integers row, cols, rCenter, and cCenter. There is a rows x cols matrix and you are on the cell with the coordinates (rCenter, cCenter).

Return the coordinates of all cells in the matrix, sorted by their distance from (rCenter, cCenter) from the smallest distance to the largest distance. You may return the answer in any order that satisfies this condition.

The distance between two cells (r1, c1) and (r2, c2) is |r1 - r2| + |c1 - c2|.

本来想的是一个BFS，后来一想用个排序就解决了。

```python
class Solution:
  def allCellsDistOrder(self, rows: int, cols: int, r: int, c: int) -> List[List[int]]:
    res = [(i, j) for i in range(rows) for j in range(cols)]
    res.sort(key=lambda x: abs(x[0]- r) + abs(x[1] - c))
    return res
```

另一个比较好的思路是，菱形遍历的方式，类似于蛇形矩阵遍历。
