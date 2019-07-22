# 207. Course Schedule

## DFS Recursive Solution
- Runtime: O(N)
- Space: O(N)
- N = Number of courses

When the problem has relationships, such as a course needing more than one prerequisite, you can think of a graph.
In this case, its a directed graph.
The core of the problem is to find a cycle in the graph, as example 2 of the problem shows that.

We will need to create a graph, as it is not provided to us, it can be an adjacent list or a matrix, doesn't matter.
For any dfs, you will need a global visited and a local visited.
The global visited will tell us if we need to dfs starting at this node, this is to reduce run-time, else it will be O(N^N).
The local visited is for when we are traversing the graph via. dfs and looking for cycles.
I decided to use a dictionary to simplify the code, -1 will be used during the dfs, then after the dfs, changed into a 1, showing that its already visited and has no cycles.

```
from collections import defaultdict

class Solution:
    def canFinish(self, numCourses: int, prerequisites: List[List[int]]) -> bool:
        adj_list = self.create_adj_list(prerequisites)
        visited = defaultdict(int)
        for node in adj_list:
            if not self.dfs(node, adj_list, visited):
                return False
        return True
        
    def dfs(self, node, adj_list, visited):
        if visited[node] == -1: # currently visiting, cycle
            return False
        if visited[node] == 1: # already visited, no cycle
            return True
        visited[node] = -1
        for neighbor in adj_list[node]:
            if not self.dfs(neighbor, adj_list, visited):
                return False
        visited[node] = 1
        return True
        
    def create_adj_list(self, prereqs):
        adj_list = defaultdict(list)
        for course, prereq in prereqs:
            adj_list[course].append(prereq)
            adj_list[prereq]
        return adj_list
```
