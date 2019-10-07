# 148. Sort List

## Merge Sort Solution
- Run-time: O(Nlog(N))
- Space: O(N)
- N = Number of list Nodes

By replicating merge sort with a linked list, we can achieve a solution.
We need to make use of the fast/slow technique to find the middle node of the list.
Once found, we can sever the link at the middle node and create two linked lists.
Then its a matter of calling merge sort on these two lists and merging them together after.
Each merge sort will return a sorted linked list, so you will end up with two sorted linked list that you need to merge and return back up the call.

```
class Solution:
    def sortList(self, head: ListNode) -> ListNode:

        def merge_sort_ll(head):
            if head is None or head.next is None:
                return head
            prev, fast, slow = None, head, head
            while fast and fast.next:
                fast = fast.next.next
                prev = slow
                slow = slow.next
            prev.next = None
            l1 = merge_sort_ll(head)
            l2 = merge_sort_ll(slow)
            return merge(l1, l2)

        def merge(l1, l2):
            curr = dummy = ListNode(0)
            while l1 and l2:
                if l1.val < l2.val:
                    l1, curr.next, curr = l1.next, l1, l1
                else:
                    l2, curr.next, curr = l2.next, l2, l2
            curr.next = l1 or l2
            return dummy.next

        return merge_sort_ll(head)
```
