# 743. Network Delay Time

## Dijkstra's Algorithm Solution
- Run-time: O(N^2)
- Space: O(N)
- N = Number of Nodes

This version of Dijkstra is fairly straightforward.
For each iteration from 1 to N.
We find the minimum distance of an unvisited vertex V.
Add it as visited and calculate if there is a better distance from K for all neighbors of V.
This can be calculated by using a dictionary of distances where the key is the node and the value is the distance, all initialized to infinite except K, which starts at 0.
We can then say that if the weight to the neighbor from V + distance of K to V is less than current shortest path of K to neighbor, then that is a shorter path.

We end up with a dictionary where each distance represents K to the node.

[Dijkstra's Algorithm Video](https://www.youtube.com/watch?v=pVfj6mxhdMw&t)

```
from collections import defaultdict

class Solution:
    def networkDelayTime(self, times: List[List[int]], N: int, K: int) -> int:

        def create_adj_list(times):
            adj_list = defaultdict(list)
            for source, to, weight in times:
                adj_list[source].append((to, weight))
            return adj_list

        distances = defaultdict(lambda: float('inf'))
        for n in range(1, N+1):
            distances[n]
        distances[K] = 0
        visited = set()
        adj_list = create_adj_list(times)
        while len(visited) != N:
            distance, vertex = min([(d, v) for v, d in distances.items() if v not in visited])
            visited.add(vertex)
            for neighbor, weight in adj_list[vertex]:
                distances[neighbor] = min(distances[neighbor], distances[vertex] + weight)
        result = max(distances.values())
        return result if result != float('inf') else -1
```

## Dijkstra's Algorithm Solution with Heaps

```
```
