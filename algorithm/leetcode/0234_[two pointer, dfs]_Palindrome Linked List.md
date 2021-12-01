`time O(n)` 这里用了stack所以空间还是n如果用了迭代就是`O(1)`了

```python
class Solution:
    def isPalindrome(self, head: Optional[ListNode]) -> bool:
        def reverse(head):
            if not head.next: return head
            h = reverse(head.next)
            head.next.next = head
            head.next = None
            return h
        
        slow, fast = head, head
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
            
        slow = reverse(slow)
        while slow and head:
            if slow.val != head.val: return False
            slow = slow.next
            head = head.next
        return True
```
