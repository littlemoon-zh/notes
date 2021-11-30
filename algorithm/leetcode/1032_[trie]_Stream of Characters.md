
Trie

```python
class StreamChecker:
  def __init__(self, words: List[str]):
    self.trie = {}
    self.str = ''
    for word in words:
      w = reversed(word)
      t = self.trie
      for ch in w:
        if ch not in t: t[ch] = {}
        t = t[ch]
      t['#'] = '#'
    
  def query(self, letter: str) -> bool:
    self.str = letter + self.str

    s = self.str
    t = self.trie
    for ch in s:
      if ch in t:
        t = t[ch]
        if '#' in t: return True
      else: return False
    return False
```
