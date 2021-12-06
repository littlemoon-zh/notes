

```python
def calculate(self, st: str) -> int:
    s = []
    sign = '+'
    num = 0
    for i, ch in enumerate(st):
        if ch.isdigit():
            num = 10 * num + ord(ch) - ord('0')

        if ch in '+-)' or i == len(st) - 1:
            if sign == '+': s.append(num)
            else: s.append(-num)
            num = 0
            sign = ch

            if ch == ')':
                cur = 0
                while s[-1] != '(':
                    cur += s.pop()
                s.pop()
                op = s.pop()
                if op == '+': s.append(cur)
                else: s.append(-cur)
                sign = '+'

        if ch == '(':
            s.append(sign)
            s.append(ch)
            sign = '+'

    return sum(s)
```
