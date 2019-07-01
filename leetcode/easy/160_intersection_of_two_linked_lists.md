# 160. Intersection of Two Linked Lists

## Best Solution
- Runtime: O(N)
- Space: O(1)
- N = Number of nodes in linked list

If you first count the amount of nodes in both linked list, you can then figure out the difference in the number of nodes between the two.
Using that, you can then move the starting pointer of the longest linked list to the position where you can iterate both linked lists at the same time to find the intersection node.

```
class Solution(object):
    def getIntersectionNode(self, headA, headB):
        """
        :type head1, head1: ListNode
        :rtype: ListNode
        """
        def get_n_nodes(root):
            if root is None:
                return 0
            n_nodes = 1
            while root is not None:
                root = root.next
                n_nodes += 1
            return n_nodes
    
        if headA is None or headB is None:
            return None
        n_A_nodes = get_n_nodes(headA)
        n_B_nodes = get_n_nodes(headB)
        n_diff_nodes = abs(n_A_nodes - n_B_nodes)
        if n_A_nodes < n_B_nodes: # headA is longest
            headA, headB = headB, headA
        for _ in range(n_diff_nodes):
            headA = headA.next
        while headA and headB:
            if headA is headB:
                return headA
            headA = headA.next
            headB = headB.next
        return None
```
