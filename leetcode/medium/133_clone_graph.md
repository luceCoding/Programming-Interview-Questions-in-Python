# 133. Clone Graph

## Dictionary and DFS Solution
- Runtime: O(V + E)
- Space: O(V)
- V = Vertices
- E = Edges

We can use a dictionary as a map to the cloned node, then by using DFS, we can figure out the cloned node in relation to the nodes in the graph.
As we visit each node with DFS, we can create a clone of each neighbor and add them to the current node's cloned node using the dictionary.
After that, we can call DFS on each neighbor.

```
class Solution:
    def cloneGraph(self, node: 'Node') -> 'Node':

        def dfs_clone(curr):
            if curr in visited:
                return
            visited.add(curr)
            for neighbor in curr.neighbors:
                if neighbor not in node_to_clone:
                    node_to_clone[neighbor] = Node(neighbor.val, [])
                node_to_clone[curr].neighbors.append(node_to_clone[neighbor])
                dfs_clone(neighbor)

        node_to_clone = dict()
        node_to_clone[node] = Node(node.val, [])
        visited = set()
        dfs_clone(node)
        return node_to_clone[node]
```
