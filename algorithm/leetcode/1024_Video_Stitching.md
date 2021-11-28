Two pointer

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
