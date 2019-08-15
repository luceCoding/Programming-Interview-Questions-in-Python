# 102. Binary Tree Level Order Traversal

## Recursive Perorder Traversal Solution with dictionary
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

## BFS Solution
- Runtime: O(N)
- Space: O(N)
- N - # nodes in tree

Similar approach, to the one above. 
However, instead of doing a preorder traversal, we traverse each level one at a time instead.

```
from collections import deque
from collections import defaultdict

class Solution:
    def levelOrder(self, root: TreeNode) -> List[List[int]]:
        if root is None:
            return list()
        queue = deque([root])
        levels = defaultdict(list)
        curr_lvl = 0
        while len(queue) > 0:
            n_pops = len(queue)
            for _ in range(n_pops):
                curr_node = queue.popleft()
                levels[curr_lvl].append(curr_node.val)
                if curr_node.left is not None:
                    queue.append(curr_node.left)
                if curr_node.right is not None:
                    queue.append(curr_node.right)
            curr_lvl += 1
        return levels.values()
```
