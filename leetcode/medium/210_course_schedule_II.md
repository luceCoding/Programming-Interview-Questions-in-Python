# 210. Course Schedule II

## Topological Sort
- Runtime: O(V + E)
- Space: O(V)
- V = Vertices
- E = Edges

There are plenty of Topological Sort explainations.
I find this one most helpful: [Toplogical Sort Video](https://www.youtube.com/watch?v=eL-KzMXSXXI&t=671s)

You can think of topological sort as an extension of DFS.

Also the question has a misleading edge case, given numCourses = 1 and prerequisites = [], expected output is [0].
They are just saying that there exists one course and that course is course 0. Essentially islands in the graph or courses with no prerequisites.

```
from collections import defaultdict

class Solution:
    def findOrder(self, numCourses: int, prerequisites: List[List[int]]) -> List[int]:
        def get_adj_list():
            adj_list = defaultdict(list)
            for course, prereq in prerequisites:
                adj_list[course].append(prereq)
            for n in range(numCourses):
                adj_list[n]
            return adj_list
        
        def top_sort(node, visited=set()):
            if node in visited: # cycle
                return False
            if node in global_visited:
                return True
            visited.add(node)
            global_visited.add(node)
            for neighbor in adj_list[node]:
                if not top_sort(neighbor):
                    return False
            ordering.append(node)
            visited.remove(node)
            return True
        
        adj_list = get_adj_list()
        global_visited = set()
        ordering = list()
        for node in adj_list:
            if not top_sort(node):
                return []
        return ordering
```
