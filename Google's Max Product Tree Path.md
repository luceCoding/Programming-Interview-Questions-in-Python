# QUESTION
Given an unsorted binary tree of numbers. Find the maximum product you can have with one continous path in the tree.

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
Then the result would be [4,2,8] which is 64.

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
print find_max_product_path_in(root)
```
