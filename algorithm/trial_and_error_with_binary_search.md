# trial and error with binary search

很多题目用trial and error的方法很容易解。这类题有个显著的特征就是，解空间的范围是有限的、可以实现计算的。比如让我们猜一个100之内的正数，只有100个可能，我们使用二分来猜，一下就可以把搜索空间减少一半。所以这类题目的另一个特征是，经过一次猜测，可以大幅缩小下次的搜索空间。比如目标数是75，我们第一次猜测50，得到反馈猜小了，于是我们的解空间从[1, 100]变成了[51, 100]，下面继续二分即可。另外一个特征是，`g(mid)`(见下面模板代码)通常比较易于计算。

通过这种题目，还可以发现二分搜索一个很好用的性质，就是可以帮助找到边界值，或者说最值。在下面这种左闭右开区间的二分搜索中，我们最终得到的值是第一次满足g()这个条件的值。所谓的第一次满足，视情况而定，比如说我们可以找第一个大于等于5的位置。

```python
while lo < hi:
  mid = (lo + hi) >> 1
  # maybe do something to figure out the value of g(mid)
  if g(mid):
    hi = mid
  else:
    lo = mid + 1
```

## LeetCode 好题

[719. Find K-th Smallest Pair Distance](https://leetcode.com/problems/find-k-th-smallest-pair-distance/)

这个题目，分析一下：
- 首先它的解空间是确定的，要找的距离一定在[0, max-min]范围内；
- 其次，可以通过二分搜索对半缩小搜索区间；
- 最后，可以较快地计算出g(mid)的值；

```java
class Solution {
    public int smallestDistancePair(int[] nums, int k) {
        Arrays.sort(nums);
        int lo = 0, hi = nums[nums.length-1] - nums[0];
        while (lo < hi) {
            int mid = (lo + hi) >> 1;
            int le = 0;
            for (int i = 0, j = 0; i < nums.length - 1; i++) {
                for (; j < nums.length; j++)
                    if (nums[j] - nums[i] > mid) break;
                le += j - i - 1;
            }
            if (le >= k)
                hi = mid;
            else
                lo = mid + 1;
        }
        return lo;
    }
}
```

[410. Split Array Largest Sum](https://leetcode.com/problems/split-array-largest-sum/)

这个题目，说实话很难想到思路，无论是dp，greedy好像都没有用武之地，但是从特征上，这题好像符合trial and error的方法。

- 解空间确定, 一定属于`[min, sum(all)]`
- 可以二分
- g(mid)好做吗，继续思考

如何计算这里的g(mid)呢？我们这里的mid，枚举的其实是子数组和中的最大的，所以为了确保它是最大的，我们要让所有的子数组和都不超过这个值，以此为条件进行划分。假设满足此条件的划分，将数组划分成t个子数组，当我们发现t比要求的数字要大时，说明我们把数组分多了，为什么会分多呢？因为限制太死了，也就是mid值小了，所以设置`lo = mid + 1`，反之，设置`hi = mid`；

代码如下：
```java
class Solution {
    public int splitArray(int[] nums, int m) {        
        int lo = Integer.MIN_VALUE;
        int hi = 0;
        for (int num : nums) {
            lo = Math.max(lo, num);
            hi += num;
        }
        
        while (lo < hi) {
            int mid = (lo + hi) >>> 1;
            int sum = 0;
            int t = 1;
            for (int num : nums) {
                sum += num;
                if (sum > mid) {
                    t++;
                    sum = num;
                }
            }
            if (t <= m)
                hi = mid;
            else
                lo = mid + 1;
        }
        return lo;
        
    }
}
```

这道题可以说是trial and error方法的精彩应用了。其他方法似乎都行不通的样子。

有些人会疑问，为什么能确保这个trial and error的值，一定是最大子数组的和。因为我们这里使用了类似于lower bound的二分技术，可以确保一定取到等号。


