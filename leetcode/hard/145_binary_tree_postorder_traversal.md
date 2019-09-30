# 145. Binary Tree Postorder Traversal

## Recursive Solution
- Runtime: O(N)
- Space: O(H)
- N = Number of elements in tree
- H = Height of tree

Post order is (Left -> Right -> Node).

The recusive solution is fairly easy. Most of the heavy lifting is abstracted away by the recursion call.

```
class Solution:
    def postorderTraversal(self, root: TreeNode) -> List[int]:
        def postorder_traversal_helper(root, result):
            if root is None:
                return
            postorder_traversal_helper(root.left, result)
            postorder_traversal_helper(root.right, result)
            result.append(root.val)
            
        result = list()
        postorder_traversal_helper(root, result)
        return result
```

## Iterative Solution
- Runtime: O(N)
- Space: O(H)
- N = Number of elements in tree
- H = Height of tree

Take a look back at how a preorder is done (Node -> Left -> Right).
Compared to postorder (Left -> Right -> Node), what are some similarities? 
You may notice that you can perform a postorder with an inverted preorder traversal.

Another way to look at it is, since postorder is (Left -> Right -> Node), we can go (Node -> Right -> Left) and reverse the result at the end to get the postorder.

So we can achieve an iterative postorder traversal via. an inverted preorder traversal.

```
class Solution:
    def postorderTraversal(self, root: TreeNode) -> List[int]:
        stack = list([root])
        inverted_preorder = list()
        while stack:
            node = stack.pop()
            if node:
                inverted_preorder.append(node.val)
                stack.append(node.left)
                stack.append(node.right)
        return inverted_preorder[::-1]
```
