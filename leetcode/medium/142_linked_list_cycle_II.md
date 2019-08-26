# 142. Linked List Cycle II

## Solution
- Runtime: O(N)
- Space: O(1)
- N = Nodes in linked list

```
class Solution(object):
    def detectCycle(self, head):
        """
        :type head: ListNode
        :rtype: ListNode
        """
        if head is None:
            return None
        slow = fast = head
        while fast is not None and fast.next is not None:
            fast = fast.next.next
            slow = slow.next
            if fast is slow:
                break
        else:
            return None
        slow = head
        while fast is not slow:
            slow = slow.next
            fast = fast.next
        return slow
```
