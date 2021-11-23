# 逆向思维

---
[828. Count Unique Characters of All Substrings of a Given String](https://leetcode.com/problems/count-unique-characters-of-all-substrings-of-a-given-string/)

[ref solution](https://leetcode.com/problems/count-unique-characters-of-all-substrings-of-a-given-string/discuss/128952/C%2B%2BJavaPython-One-pass-O(N))

```python
def uniqueLetterString(self, s: str) -> int:
    c = {ch : [-1, -1] for ch in string.ascii_uppercase}
    res = 0
    for i, ch in enumerate(s):
        a, b = c[ch]
        res += (i - b) * (b - a)
        c[ch] = [b, i]

    for k, (a, b) in c.items():
        res += (len(s) - b) * (b - a)
    return res
```

---
[401. Binary Watch](https://leetcode.com/problems/binary-watch/)

可以使用dfs来搜索，但是更简单的方法是从结果出发，把所有可能方案列出来，看哪些符合条件。因为搜索空间比较小，所以这样做比较快。
