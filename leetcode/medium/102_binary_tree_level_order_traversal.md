# 102. Binary Tree Level Order Traversal

## Solution
- Runtime: O(N)
- Space: O(N)
- N - # nodes in tree

We essentially need to visit the left side of the tree first to find the elements to create each level before we visit the right side.
Then we need a dictionary of lists to store these levels, each level will contain a list of elements.
So as we traverse in a preorder traversal fashion, we store the values into the dictionary.

```
from collections import defaultdict

class Solution:
    def levelOrder(self, root: TreeNode) -> List[List[int]]:
        def level_order_helper(root, curr_level, level_map):
            if root is None:
                return
            level_map[curr_level].append(root.val)
            level_order_helper(root.left, curr_level+1, level_map)
            level_order_helper(root.right, curr_level+1, level_map)
            
        level_map = defaultdict(list)
        level_order_helper(root, 0, level_map)
        return level_map.values()
```
