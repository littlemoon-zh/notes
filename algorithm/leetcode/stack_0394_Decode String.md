
分析，找到规律，使用栈。

```python
def decodeString(self, st: str) -> str:
    s = []
    num = 0
    for ch in st:
        if ch.isdigit():
            num = 10 * num + ord(ch) - ord('0')
        elif ch == '[':
            s.append(num)
            s.append(ch)
            num = 0
        elif ch == ']':
            cur = ''
            while s[-1] != '[':
                cur =  s.pop() + cur
            s.pop(-1)
            s.append(cur * s.pop())
        else:
            s.append(ch)

    return ''.join(s)
```
