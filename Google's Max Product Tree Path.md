# QUESTION
Given an unsorted binary tree of numbers. Find the maximum product you can have with one continous path in the tree. You may not modify the tree.

For example:
```
        4
      /   \
     1     2
    / \   / \
   3   5 8   6
```
The result would be the product of [5,1,4,2,8] which is 340.

Similarly:
```
        4
      /   \
     0     2
    / \   / \
   3   5 8   6
```
Then the result would be [8,2,6] which is 96.

# EXPLAINATION
This is a very good problem to ask a person that has never seen this question. This really figures out how a person problem solves.
This was how I approached the problem. 

First, I took the perspective of a node in the tree. What does a node need from its children? Let's use a more diluted example to explain this.

```
        4
      /  
     1   
    / \  
   3   5 
```
You can see that, [5,1,4] is the final result. But as node 1 you believe that [3,1,5] is the best result. Obviously, if node 4 was instead node 0, [3,1,5] would be the final result. So how do we choose? 

As a node, all you really care about is who the best left and right product. From there you can use those two and your node's value to figure out the best product. But as you've seen above, you need more than that. You also need to know who is the best left product that you can connect with your node as well as the best left product that is not connectable to create a path with your node, similar case for the right. Also consider the case, where you sum the products of your node and connectable left and right to create a product is that not connectable by a parent node.

# SOLUTION
```
def find_max_product_path_in(root):
    if not root:
        return 0
    result1, result2 = find_max_product_path_helper(root)
    return max(result1, result2)
    
def find_max_product_path_helper(root):
    if not root.left and not root.right:
        return root.val, 0
    left_connectable_product, left_nonconnectable_product = 0, 0
    right_connectable_product, right_nonconnectable_product = 0, 0
    if root.left:
        left_connectable_product, left_nonconnectable_product = find_max_product_path_helper(root.left)
    if root.right:
        right_connectable_product, right_nonconnectable_product = find_max_product_path_helper(root.right)
        
    best_connectable_product, best_nonconnectable_product = root.val, 0
    
    best_connectable_product = max(best_connectable_product, left_connectable_product * root.val)
    best_connectable_product = max(best_connectable_product, right_connectable_product * root.val)
    
    best_nonconnectable_product = max(best_nonconnectable_product, left_nonconnectable_product)
    best_nonconnectable_product = max(best_nonconnectable_product, right_nonconnectable_product)
    new_product = root.val * left_connectable_product * right_connectable_product
    best_nonconnectable_product = max(best_nonconnectable_product, new_product)
    
    return best_connectable_product, best_nonconnectable_product
        
class Node:
    def __init__(self, value, left, right):
        self.val = value
        self.left = left
        self.right = right

node3 = Node(3, None, None)
node5 = Node(5, None, None)
node1 = Node(1, node3, node5)
node8 = Node(8, None, None)
node6 = Node(6, None, None)
node2 = Node(2, node8, node6)
root = Node(4, node1, node2)
print find_max_product_path_in(root) # return 320

node0 = Node(0, node3, node5)
root = Node(4, node0, node2)
print find_max_product_path_in(root) # returns 96

node0A = Node(0, None, None)
node0B = Node(0, None, None)
root = Node(4, node0A, node0B)
print find_max_product_path_in(root) # returns 4
```
