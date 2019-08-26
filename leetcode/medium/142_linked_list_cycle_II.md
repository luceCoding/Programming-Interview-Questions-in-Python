# 142. Linked List Cycle II

## Solution
- Runtime: O(N)
- Space: O(1)
- N = Nodes in linked list

By using a fast and slow pointer, slow moves once while fast moves twice, we can figure out if there is a cycle.
However, there is a second property in doing this, at the point where slow and fast meet in the cycle, it is exactly K steps away from the start of the cycle as the head is as fast is to the beginning of the cycle.

Reason why this works is that, no matter where the cycle begins or if the number of nodes is even or odd: 
1. Slow and fast will always end up moving an even number of times. 
2. Fast will always move twice as much as slow. 
3. Fast would have at least went around the cycle one time. 

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
