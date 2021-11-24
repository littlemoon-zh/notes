# classic search and backtracking problem(using DFS)

搜索问题，在我看来是最重要、最基础的算法，是通往一切高级算法的起点。思想就是在搜索空间上挨个尝试，很多算法问题本质上都是搜索问题，只不过未必显式使用dfs或者bfs。

- 搜索问题，通常用dfs/bfs来解决，有时两种方法是interchangeable的。dfs用递归实现的多一些，而bfs几乎只用队列来实现。
- dfs用于找出符合规定的解，bfs通常还可以找出**最值问题**，比如路径权重为1时，最短路径问题就退化成bfs问题。因为bfs是从近到远一次遍历，所以当它访问到某个位置，一定是距离原点最近的可达位置。
- 在很多搜索问题中，为了防止重复访问，会使用一个辅助结构来记录是否访问过某个位置，比如一个数组，或者set。很多情况，在回溯时，要清除当前这一步对这个辅助结构的影响，否则得不到正确的答案。
不过有时候，不需要清除。比如你想知道是否有一条路径可以通向终点，那么这里dfs就不需要清除路径，因为这只是一个存在性问题，你并不想知道所有路径。比如 [778 Swim in Rising Water](https://leetcode.com/problems/swim-in-rising-water/)
- 关于返回值，有时候dfs返回值可以设置成True或False，表示是否找到解，这样一来，在找到答案后就不需要进行后续的搜索。
- 对于难一点的题目，dfs可能只是其中一个验证的步骤，比如用于二分中，对mid值进行一些验证；又或者dfs中验证某一步是否valid的函数比较复杂；
- 对于一些题目要考虑搜索顺序的问题，比如从中间搜，从起始点搜，还是根据目标来搜，有些情况下，搜索顺序起到至关重要的作用。比如 [401. Binary Watch](https://leetcode.com/problems/binary-watch/)


下面是LeetCode上一些经典的搜索问题：

## [37 Sudoku Solver](https://leetcode.com/problems/sudoku-solver/)

从这题可以看出，搜索的顺序也是因题而异。对于大部分matrix类型的搜索，都是遍历上下左右四个方向。但是这里填迷宫问题，最好还是按行遍历。

即使是按行遍历，也有多种写法，比如你可以再每个dfs中写一个二层循环，每次遇到第一个需要填的点，就dfs，这就很自然形成了一个从左到右、从上到下的遍历方式。

或者手动控制这种按行遍历，如果当前是`(x, y)`那么下一步要么是`(x, y+1)`要么是`(x+1, 0)`。

这题需要清除visited状态，因为后面的结果会对当前产生干扰。

这里对迷宫valid的判断也相对复杂。

## [542 01 Matrix](https://leetcode.com/problems/01-matrix/)

这题是要找出对每个位置而言，找到离它最近的0的距离。提到了**最近**，很自然地想到用bfs来做。当然可以对每个1元素做一次bfs，但是这样会超时。

所以这题是比较高级的bfs，首先把所有的0入队列，因为他们距离0的距离是0，接着0的周围的所有没遍历过的节点入队，它们到0的距离必然是1，下面再重复这个过程，就一次得到距离为2，3，4...的所有点。

这里使用bfs+visited可以保证，对整个图只遍历一次，但是使用逐点bfs就会大大增加搜索次数，假设一个图里面只有一个点是0，那么远离这个0的点进行bfs时就相当于遍历整个图。


## [698. Partition to K Equal Sum Subsets](https://leetcode.com/problems/partition-to-k-equal-sum-subsets/)

非常好的一个题。虽然是搜索题，但是找到如何搜索，如何高效搜索还是比较困难的。比如有时候从目标出发向起点搜索比较好，有时候从起点向终点扩张比较好，要灵活选用。

这题搜索的方向是，能满足一个先满足一个，满足这一个再看下一个。我原本的是搜索方法是，把这些数先挨个分给这些k个子数组，不满足的时候再进行回溯，也就是我同时搜索了多个。这可能是我速度很慢的原因。


## [778 Swim in Rising Water](https://leetcode.com/problems/swim-in-rising-water/) 二分+搜索

比较好的题。由于我们没有什么方式可以确定time，所以干脆就挨个试一试，使用binary search的框架可以增加搜索速度。

这题的dfs不需要进行回溯，假设有一条通向终点的路径，那么一定可以通过dfs到达，不需要清除之前的状态。

```python
class Solution:
    def swimInWater(self, g: List[List[int]]) -> int:
        n = len(g)
        lo = 0
        hi = n * n
        
        def dfs(x, y, bound, used):
            if x == n-1 and y == n-1:
                if g[x][y] <= bound: return True
                else: return False
            if g[x][y] > bound: return False
            used[x][y] = True
            
            for dx, dy in [[-1,0], [1,0], [0,1], [0,-1]]:
                xx = x + dx
                yy = y + dy
                if xx >= 0 and xx < n and yy >= 0 and yy < n and not used[xx][yy]:
                    if dfs(xx, yy, bound, used):
                        return True
            # used[x][y] = False
            return False
        
        def valid(bound):
            used = [[0] * n for i in range(n)]
            return dfs(0, 0, bound, used)
        
        while lo < hi:
            mid = (lo + hi) >> 1
            
            if valid(mid):
                hi = mid
            else:
                lo = mid + 1
        
        return lo
```

## [980. Unique Paths III](https://leetcode.com/problems/unique-paths-iii/)

也是一个路径搜索问题，dfs+回溯可以解决。

## [52 N-Queens II](https://leetcode.com/problems/n-queens-ii/)

经典N皇后问题，没什么可说的，相对来讲比较常规，判断是否valid也比较直观。因为每行只能放一个数，所以可以在每一行的几个列中选取一个位置，如果可以放，就继续到下一行进行递归，直到最后一行。

## [140. Word Break II](https://leetcode.com/problems/word-break-ii/)

Not bad. Compared with dynamic programming one: [139. Word Break](https://leetcode.com/problems/word-break/)

DP vs Search for these two problems:
- DP has much more data input, i.e. n=300, while search only has n=20.
- Search requires all the possible solution, DP requires the existence.

```python
def wordBreak(self, s: str, wordDict: List[str]) -> List[str]:
    res = []
    def dfs(s, path):
        if s == '':
            res.append(' '.join(path))
            return
        for i in range(len(s) + 1):
            if s[:i] in wordDict:
                dfs(s[i:], path + [s[:i]])
    dfs(s, [])
    return res
```

## [39. Combination Sum](https://leetcode.com/problems/combination-sum/)

Not bad. Complete bag.

```python
def dfs(start, cur, path):
    # some code here
    
    # keep using the current
    dfs(start, cur + nums[start], path + [nums[start]])
    # then the next
    dfs(start + 1, cur, path)
```
