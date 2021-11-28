## [239 Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/)


这题的要返回的是在滑动窗口中的最大值。为了最大值，肯定涉及到有序之类的东西，不然每次滑动都遍历窗口找最大值的话，显得有些低效。

单调队列是这题的最好方法。单调队列是指，队列中的元素具有某种单调性。如果我们有维护单调递减队列，那么每次窗口的最大值就是第一个元素了。此外，队列长度小于等于窗口长度，而不是一定会等于，因为最大值可能出现在中间某个位置，而我们已经把之前的弹出了。

这里需要的是一个双端队列，我们需要同时能在队头和队尾进行操作。

```java
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        Deque<Integer> q = new LinkedList<>();
        int[] res = new int[nums.length - k + 1];
        for (int i = 0; i < nums.length; i++) {
            if (!q.isEmpty() && i - q.getFirst() >= k) 
                q.pollFirst();
            
            while (!q.isEmpty() && nums[i] > nums[q.getLast()])
                q.pollLast();
            q.addLast(i);
            if (i >= k-1)
                res[i-k+1] = nums[q.getFirst()];
        }
        return res;
    }
}
```
