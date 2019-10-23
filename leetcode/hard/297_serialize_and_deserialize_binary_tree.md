# 297. Serialize and Deserialize Binary Tree

## DFS Solution
- Run-time: O(N)
- Space: O(N)
- N = Number of nodes in tree

We cannot reuse the solutions from question #105. or #106 using two traversals because duplicate values are allowed.
We would need a different method.

A DFS method is probably the easiest method to come up with.
I chose to use preorder, you can implement this using any of the traversals.

Let's start with serialize(), we can simply traverse via. preorder and construct a list.
This part is straightforward, however, there is a caveat and that is related to space and how we should represent None nodes.
We could use a string like 'None' but we can save more space by using an empty string instead, this would require us to use a delimiter.

For deserialize(), using the delimiters, we can get the list of nodes in preorder.
It would simply be a preorder traversal to reconstruct the tree.

There are some further optimizations that we could've done, like condensing consecutive empty strings together.
It would be more over engineering at this point but good to mention.

```
class Codec:

    curr_idx = 0
    def serialize(self, root):
        """Encodes a tree to a single string.

        :type root: TreeNode
        :rtype: str
        """
        def preorder_encode(root):
            if root is None:
                result.append('')
                return
            result.append(str(root.val))
            preorder_encode(root.left)
            preorder_encode(root.right)

        result = list()
        preorder_encode(root)
        return ','.join(result)

    def deserialize(self, data):
        """Decodes your encoded data to tree.

        :type data: str
        :rtype: TreeNode
        """
        def preorder_decode():
            if self.curr_idx > len(tokens) or tokens[self.curr_idx] == '':
                return None
            root = TreeNode(int(tokens[self.curr_idx]))
            self.curr_idx += 1
            root.left = preorder_decode()
            self.curr_idx += 1
            root.right = preorder_decode()
            return root

        tokens = data.split(',')
        return preorder_decode()
```
