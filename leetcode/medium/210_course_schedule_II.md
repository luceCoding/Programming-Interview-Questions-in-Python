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

class Solution(object):
    def findOrder(self, numCourses, prerequisites):
        
        def get_graph(n_courses, prereqs):
            graph = defaultdict(list)
            for course, prereq in prereqs:
                graph[course].append(prereq)
            for n in range(n_courses):
                graph[n]
            return graph
            
        def dfs(node, global_visited, visited, graph, ordering):
            if node in visited: # cycle detected
                return False
            if node in global_visited:
                return True
            visited.add(node)
            global_visited.add(node)
            for neighbor in graph[node]:
                if not dfs(neighbor, global_visited, visited, graph, ordering):
                    return False
            visited.remove(node)
            ordering.append(node)
            return True
        
        graph = get_graph(numCourses, prerequisites)
        ordering = list()
        global_visited = set()
        for node in graph:
            if node not in global_visited:
                if not dfs(node, global_visited, set(), graph, ordering):
                    return []
        return ordering
```
