### Finding the middle node
To find the middle node, it is best to create two node pointers. One that is fast and one that is slow. The fast pointer will always move two steps while the slow pointer moves one step at a time.
You will continue to evaluate the fast pointer if it reaches null to know when to stop. After that, you can use the slow pointer as the middle node.

### Trick to implementing a Doubly Linked List
As you may have seen when implementing any linked list there are quite a few cases to consider. When removing or adding to a linked list there are cases like:
- Is the list currently empty? 
- Is the target element at the very end of the list? 
- Is the target element in the middle of the list? 
There is a small trick to avoid checking all these cases and its by using a ciruclar linked list with a dummy node as the head. You will never remove the dummy node nor add a new dummy node. This dummy node is guaranteed, so it can eliminate a lot of cases off the bat.
