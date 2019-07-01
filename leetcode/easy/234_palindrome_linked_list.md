# 234. Palindrome Linked List

## Best Solution
- Runtime: O(N)
- Space: O(1)
- N = Number of nodes in linked list

I believe many folks over complicate things with this question.
Take these two examples:
```
1->2->2->1
   ^
   
1->2->3->2->1
      ^
```
If you found the middle node, you could then reverse the linked list from that middle node creating the following:
```
1->2<-2<-1
1->2->3<-2<-1
```
Notice how the linked list with the even number of nodes has two nodes pointing to it. Does it matter? Not at all.
By having two pointers on either end of the linked list after the reversal, you can then iterate and easily compare their values.
Essentially solving the problem in two passes. Make sure you ask the interviewer if you can modify the original linked list.

```
class Solution:
    def isPalindrome(self, head: ListNode) -> bool:
        def get_mid_node(head):
            slow = fast = head
            while fast and fast.next:
                slow = slow.next
                fast = fast.next.next
            return slow
        
        def reverse_ll(head):
            prev = None
            while head is not None:
                curr = head
                head = head.next
                curr.next = prev
                prev = curr
            return prev
        
        def is_ll_palindrome(L1, L2):
            while L1 and L2:
                if L1.val != L2.val:
                    return False
                L1, L2 = L1.next, L2.next
            return True
        
        mid_node = get_mid_node(head)
        L2 = reverse_ll(mid_node)
        return is_ll_palindrome(head, L2)
```
