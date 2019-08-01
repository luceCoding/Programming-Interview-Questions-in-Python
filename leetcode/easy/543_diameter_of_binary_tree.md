# 543. Diameter of Binary Tree

## Best Solution
- Runtime: O(N)
- Space: O(D)
- N = Number of elements in tree
- D = Depth of Tree

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

Connectable path means taking your child's connectable path and connecting it with yourself, hence, creating a longer chain.
This is completely separate from the curent longest path. 
You want to know if you can create a longer chain than the current longest path found so far by adding the node you are currently on and the longest paths from the right and left that also include themselves as part of the chain.

```
class Solution:
    def __init__(self):
        self.longest = 0
    
    def diameterOfBinaryTree(self, root: TreeNode) -> int:
        if root is None:
            return 0
        longest_connectable_path = self._get_diameter_helper(root)
        return self.longest-1
    
    def _get_diameter_helper(self, root):
        if root is None:
            return 0
        n_right_connectable_nodes = self._get_diameter_helper(root.right)
        n_left_connectable_nodes = self._get_diameter_helper(root.left)
        n_connected_nodes = n_right_connectable_nodes + n_left_connectable_nodes + 1
        self.longest = max(self.longest, n_connected_nodes)
        return max(n_right_connectable_nodes, n_left_connectable_nodes)+1
```

# Follow-up Question
Return instead the values of the nodes that are the longest. 
With the above example, return a list containing [4,2,1,3] and [5,2,1,3].
