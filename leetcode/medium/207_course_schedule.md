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
The global visited will tell us if we need to dfs starting at this node, this is to reduce run-time, else it will be O(N^2).
The local visited is for when we are traversing the graph via. dfs and looking for cycles.

```
from collections import defaultdict

class Solution:
    def canFinish(self, numCourses: int, prerequisites: List[List[int]]) -> bool:
        def create_graph():
            graph = defaultdict(list)
            for course, prereq in prerequisites:
                graph[course].append(prereq) 
                graph[prereq]
            return graph
            
        def dfs(course, graph, visited, global_visited):
            if course in visited:
                return False # found cycle
            if course in global_visited:
                return True
            visited.add(course)
            global_visited.add(course)
            for prereq in graph[course]:
                if not dfs(prereq, graph, visited, global_visited):
                    return False
            visited.remove(course)
            return True
        
        graph = create_graph() # key: course, val: list of prereqs
        global_visited = set()
        for course in graph:
            if not dfs(course, graph, set(), global_visited): # cycle
                return False
        return True
```
