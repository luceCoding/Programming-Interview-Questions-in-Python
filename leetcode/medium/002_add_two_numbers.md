# 2. Add Two Numbers

## Best Solution
- Runtime: O(L1) + O(L2)
- Space: O(L1) + O(L2)
- L1 - Nodes in list 1
- L2 - Nodes in list 2

The only tricky part is the carry over.
You could implement it with a boolean for the carry over, however, I believe it creates more if else statements.
Instead, if you used a rolling sum instead and removed the last digit of the sum, it will be more elegant.

```
class Solution:
    def __init__(self):
        self._sum = 0
        self._dummy_head = ListNode(-1)
        self._tail = self._dummy_head
    
    def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
        while l1 and l2:
            digit = self.get_digit(l1.val+l2.val)
            self.add_new_node(digit)
            l1, l2 = l1.next, l2.next
        while l1:
            digit = self.get_digit(l1.val)
            self.add_new_node(digit)
            l1 = l1.next
        while l2: 
            digit = self.get_digit(l2.val)
            self.add_new_node(digit)
            l2 = l2.next
        if self._sum != 0:
            self.add_new_node(1)
        return self._dummy_head.next
            
    def get_digit(self, _sum):
        self._sum += _sum
        digit = self._sum % 10
        self._sum //= 10
        return digit
    
    def add_new_node(self, digit):
        new_node = ListNode(digit)
        self._tail.next = new_node
        self._tail = new_node
```
