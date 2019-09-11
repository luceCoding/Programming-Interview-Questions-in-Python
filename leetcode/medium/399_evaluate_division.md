# 399. Evaluate Division

## DFS Solution
- Runtime: O(N)
- Space: O(N)
- N = Number of unique nodes

The first intuition is to see this relationship, if A/B = 2 then B/A = 1/2.
This is rather simple math.
Once this is noticed, you can see that there is a realtionship pattern here and a graph approach is possible.
After that you can build a graph out of the equations and values given and perform a DFS or BFS for the solution.

```
from collections import defaultdict

class Solution:
    def calcEquation(self, equations: List[List[str]], values: List[float], queries: List[List[str]]) -> List[float]:
        
        def create_graph():
            graph = defaultdict(dict)
            for equation, val in zip(equations, values):
                start, end = equation
                graph[start][end] = val
                graph[end][start] = 1.0 / val
            return graph
        
        def dfs(graph, start, end, visited):
            if start in visited or start not in graph:
                return -1.0
            if start == end:
                return 1.0
            visited.add(start)
            for neighbor, val in graph[start].items():
                result = dfs(graph, neighbor, end, visited)
                if result > 0:
                    return result * val
            return -1.0
        
        graph = create_graph()
        results = list()
        for start, end in queries:
            results.append(dfs(graph, start, end, set()))
        return results
```

## BFS Solution
- Runtime: O(N)
- Space: O(N)
- N = Number of unique nodes

```
from collections import deque
from collections import defaultdict

class Solution:
    def calcEquation(self, equations: List[List[str]], values: List[float], queries: List[List[str]]) -> List[float]:
        def create_graph():
            graph = defaultdict(dict)
            for eq, val in zip(equations, values):
                start, end = eq
                graph[start][end] = val
                graph[end][start] = 1.0 / val
            return graph
        
        def bfs(start, end, results):
            queue = deque([(start, 1.0)])
            visited = set([start])
            while len(queue) != 0:
                node, curr_prod = queue.pop()
                if node not in graph:
                    continue
                if node == end:
                    results.append(curr_prod)
                    break
                for neighbor, val in graph[node].items():
                    if neighbor not in visited:
                        visited.add(neighbor)
                        queue.appendleft((neighbor, curr_prod * val))
            else:
                results.append(-1.0)

        graph = create_graph()
        results = list()
        for start, end in queries:
            bfs(start, end, results)
        return results
```
