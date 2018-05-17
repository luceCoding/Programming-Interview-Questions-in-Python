### Finding the middle node
To find the middle node, it is best to create two node pointers. One that is fast and one that is slow. The fast pointer will always move two steps while the slow pointer moves one step at a time.
You will continue to evaluate the fast pointer if it reaches null to know when to stop. After that, you can use the slow pointer as the middle node.
