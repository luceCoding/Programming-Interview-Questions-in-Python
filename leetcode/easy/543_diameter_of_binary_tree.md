# 543. Diameter of Binary Tree

## Best Solution
- Runtime: O(N)
- Space: O(N)
- N = Number of nodes in binary tree

Take this binary tree example:
```
    1
   / \
  2   3
 / \
4   5
```

In the perspective of node 1, what do you need? 
- You will need to know what the current longest path found so far from either left or right is.
- You will also need to know what the longest 'connectable' path is.

Connectable path means taking your child's connectable path and connecting it with yourself, hence, creating a long chain.
This is completely separate from the curent longest path. 
You want to know if you can create a longer chain than the current longest path found so far.

```
class Solution:
    def diameterOfBinaryTree(self, root: TreeNode) -> int:
        def length_helper(root):
            if root is None:
                return 0, 0
            # Figure out the longest right and left
            # Then see if we can use either right or left to create a longer path
            longest_right, n_right_connectable_nodes = length_helper(root.right)
            longest_left, n_left_connectable_nodes = length_helper(root.left)
            n_connected_nodes = n_right_connectable_nodes + n_left_connectable_nodes + 1
            local_longest = max(longest_right, longest_left, n_connected_nodes)
            best_n_connectable_nodes = max(n_right_connectable_nodes+1, n_left_connectable_nodes+1)
            return local_longest, best_n_connectable_nodes
        if root is None:
            return 0
        longest_path, longest_connectable_path = length_helper(root)
        return longest_path-1
```
