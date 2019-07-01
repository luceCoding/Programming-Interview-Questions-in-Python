# 617. Merge Two Binary Trees

## Recursive Solution

- Runtime: O(N)
- Space: O(N)
- N = Number of elements in both trees

By traversing both trees together, the solution is much simpler. 
Use one of the trees as your primary merge tree during the recursion and create new nodes when the primary doesn't have one and the secondary tree does.
With recursion, you need to create the new nodes when you are the parent before you perform a function call into that node.
You cannot create the new node if the node doesn't exist after the call.

Recursion always has O(N) at the very least.

```
class Solution:
    def mergeTrees(self, t1: TreeNode, t2: TreeNode) -> TreeNode:
        def merge_tree_helper(T1, T2):
            if T1 is None or T2 is None:
                return
            T1.val += T2.val #T1 is the primary tree
            if T2.left is not None and T1.left is None:
                T1.left = TreeNode(0)
            if T2.right is not None and T1.right is None:
                T1.right = TreeNode(0)
            merge_tree_helper(T1.left, T2.left)
            merge_tree_helper(T1.right, T2.right)
        
        if t1 is None:
            return t2
        merge_tree_helper(t1, t2)
        return t1
```

## Iterative Solution

- Runtime: O(N)
- Space: O(N)
- N = Number of elements in both trees

Similar to the recursion solution, however, will need to keep two items in the each element of the stack since the idea was to traverse the nodes in pairs. You could use two stacks but I believe the code would be more clunky.

```
class Solution:
    def mergeTrees(self, t1: TreeNode, t2: TreeNode) -> TreeNode:
        if t1 is None:
            return t2
        stack = list()
        stack.append((t1, t2))
        while len(stack) > 0:
            node1, node2 = stack.pop()
            if node1 is None or node2 is None:
                continue
            node1.val += node2.val
            if node2.left is not None and node1.left is None:
                node1.left = TreeNode(0)
            if node2.right is not None and node1.right is None:
                node1.right = TreeNode(0)
            stack.append((node1.left, node2.left))
            stack.append((node1.right, node2.right))
        return t1
```
