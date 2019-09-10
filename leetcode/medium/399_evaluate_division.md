# 399. Evaluate Division

## DFS Recursive Solution
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
        
        def create_adj_list():
            adj_list = defaultdict(dict)
            for equation, val in zip(equations, values):
                start, end = equation
                adj_list[start][end] = val
                adj_list[end][start] = 1.0 / val
            return adj_list
        
        def dfs(adj_list, start, end, visited):
            if start in visited or start not in adj_list:
                return -1.0
            if start == end:
                return 1.0
            visited.add(start)
            for neighbor, val in adj_list[start].items():
                result = dfs(adj_list, neighbor, end, visited)
                if result > 0:
                    return result * val
            return -1.0
        
        adj_list = create_adj_list()
        results = list()
        for start, end in queries:
            results.append(dfs(adj_list, start, end, set()))
        return results
```
