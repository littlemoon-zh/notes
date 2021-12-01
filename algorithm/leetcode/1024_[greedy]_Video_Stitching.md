You are given a series of video clips from a sporting event that lasted time seconds. These video clips can be overlapping with each other and have varying lengths.

Each video clip is described by an array clips where clips[i] = [starti, endi] indicates that the ith clip started at starti and ended at endi.

We can cut these clips into segments freely.

For example, a clip [0, 7] can be cut into segments [0, 1] + [1, 3] + [3, 7].
Return the minimum number of clips needed so that we can cut the clips into segments that cover the entire sporting event [0, time]. If the task is impossible, return -1.

 

Example 1:
```
Input: clips = [[0,2],[4,6],[8,10],[1,9],[1,5],[5,9]], time = 10
Output: 3
Explanation: 
We take the clips [0,2], [8,10], [1,9]; a total of 3 clips.
Then, we can reconstruct the sporting event as follows:
We cut [1,9] into segments [1,2] + [2,8] + [8,9].
Now we have segments [0,2] + [2,8] + [8,10] which cover the sporting event [0, 10].
```


## Greedy + Two pointer

贪心策略：在能够到达的范围中，选择右端点眼神最远的。

```python
class Solution:
    def videoStitching(self, clips: List[List[int]], time: int) -> int:
        clips.sort()
        if clips[0][0] != 0: return -1
        last = 0
        i = 0
        res = 0
        while i < len(clips):
            if clips[i][0] > last: return -1
            r = last
            while i < len(clips) and clips[i][0] <= last:
                r = max(r, clips[i][1])
                i += 1
            res += 1
            last = r
            
            if last >= time:
                break
                
        if last >= time:
            return res
        return -1
```
