
迭代：
```python
def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
  cur = head
  pre = None
  while cur:
    nxt = cur.next
    cur.next = pre
    pre = cur
    cur = nxt
  return pre
```

递归：
```python
def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
    def dfs(head):
        if not head.next: 
            return head
        h = dfs(head.next)
        head.next.next = head
        head.next = None
        return h
    return dfs(head)
```
