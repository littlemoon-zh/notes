

Inplace modify.

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next

def reorderList(self, head: Optional[ListNode]) -> None:
    """
    Do not return anything, modify head in-place instead.
    """
    def reverse(root):
        if not root.next: return root
        h = reverse(root.next)
        root.next.next = root
        root.next = None
        return h

    # find mid
    slow, fast = head, head
    pre = None
    while fast and fast.next:
        pre = slow
        slow = slow.next
        fast = fast.next.next
    if pre: pre.next = None
    else: return head

    # reverse the second half
    a, b = head, reverse(slow)

    # connect both parts
    cur = dummy = ListNode(-1)
    switch = 1
    while a and b:
        if switch:
            cur.next = a
            a = a.next
        else:
            cur.next = b
            b = b.next
        cur = cur.next
        switch = switch ^ 1
    if b: cur.next = b
```
