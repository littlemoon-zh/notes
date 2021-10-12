# quick sort

## Partition

决定快速排序性能的是partition的好坏，如果partition正确、高效、公平，那么快排才可以保证高性能。

### 双指针两边压缩
假定我们要实现一个ascending order。下面这种压缩，取left元素作为pivot；并且初始化`i=l, j=r+1`，在循环中，使用前缀的`++`和`--`，并且使用严格的`> <`是为了保证所有元素相等是，可以产生相等的划分。

在退出时，为何是将l与j进行交换呢？从循环退出之前的最后一次，主要有两种情况；

一种是： `i, x, j`

另一种是 `i, j`

无论哪一种方式，也无论x与pivot的关系如何，最终退出时，都有nums[j] <= pivot, nums[i] >= pivot，假设和i位置交换，有可能把一个大的元素交换给left位置了，这是不正确的。

此外在退出循环后再做一次交换的目的是，将pivot放到正确的位置，如果不做这一步，有可能会导致死循环，得不到正确结果。

```java
public int partition(int[] nums, int l, int r) {
    if (l == r)
        return r;
    int i = l, j = r + 1;
    int pivot = nums[l];
    while (i < j) {
        while (nums[++i] < pivot) if(i == r) break;
        while (nums[--j] > pivot) if(j == l) break;
        if (i < j) swap(nums, i, j);
    }
    swap(nums, j, l);
    return j;
}
```
 ### 单边扩展法
 
 另一种比较好理解的partition方法是单边扩展法，维护一个border指针，在这个位置之前的，都是小于等于pivot，从而将元素划分出来。
 
 ```java
 int partition(int[] a, int lo, int hi){
    int i = lo - 1, j = lo;
    int t, pivot = a[hi];
    while (j < hi) {
        if (a[j] < pivot) {
             i++; //border of smaller elements
             t = a[j];
             a[j] = a[i];
              a[i] = t;
          }
            j++;
        }
        // i+1 is the positon
        t = a[hi];
        a[hi] = a[i + 1];
        a[i + 1] = t;
        return i + 1;
}
 ```

## 相关问题

[215. Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/)
