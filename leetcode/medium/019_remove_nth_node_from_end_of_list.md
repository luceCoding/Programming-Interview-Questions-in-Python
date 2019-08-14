# 19. Remove Nth Node From End of List

## Two pointer solution

- Runtime: O(N)
- Space: O(1)
- N = Number of nodes in linked list

The first solution you may come up with is to do a two pass solution, where you count how many nodes are in the linked list then traverse a second time to get the correct node.

However, if you were to use a two pointers, you can do this in one pass.
Simply start with a pointer that moves N nodes forward.
Then move both pointers simultaneously until the first pointer reaches the last node.
The second pointer that was behind the first will be at the position before the node that needs to be deleted.

Another thing to consider is when you need to delete the head node.
A simple way to remove extra if else cases is to incorporate a dummy node.

```
class Solution:
    def removeNthFromEnd(self, head: ListNode, n: int) -> ListNode:
        dummy_head = ListNode(0)
        dummy_head.next = head
        front = back = dummy_head
        for _ in range(n):
            front = front.next
        while front.next is not None:
            front = front.next
            back = back.next
        node_to_del = back.next
        back.next = back.next.next
        del node_to_del
        return dummy_head.next
```
