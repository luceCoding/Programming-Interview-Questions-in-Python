# 743. Network Delay Time

## Simple Dijkstra's Algorithm Solution
- Run-time: O(V^2)
- Space: O(V)
- V = Number of Vertices

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

- Run-time: O((V + E)logV)
- Space: O(V)
- V = Vertices
- E = Edges

As explained in question #1135, there is no adaptable heap implementation in Python 3.7.
Instead, we will be using a lazy deletion method, similar to question #1135.

The only difference between Prim vs. Dijkstra is adding/updating with the previous weights in the heap nodes.
In Prim's we only care about the next weight.
In Dijkstra, since we want the shortest paths, we need to reevaluate the weights by checking if we can produce an even smaller weight.
Therefore, we need to compare between the shortest path to neighboring vertex vs. the current vertex's shortest path + current weight to neighboring vertex.

```
from collections import defaultdict

class Solution:
    def networkDelayTime(self, times: List[List[int]], N: int, K: int) -> int:

        def create_adj_list():
            adj_list = defaultdict(list)
            for source, target, time in times:
                adj_list[source].append([target, time])
            return adj_list

        def add_target(target, time):
            new_node = [time, target, False]
            target_to_heap_node[target] = new_node
            heapq.heappush(min_heap, new_node)

        adj_list = create_adj_list()
        start_node = [0, K, False] # time, source, remove
        target_to_heap_node = dict({K: start_node}) # key=target, val=time
        min_heap = list([start_node])
        times = defaultdict(lambda: float('inf')) # key=source, val=time
        times[K] = 0
        visited = set()
        while min_heap:
            time, source, remove = heapq.heappop(min_heap)
            if remove: # lazy delete
                continue
            visited.add(source)
            for next_target, t in adj_list[source]:
                if next_target in visited:
                    continue
                next_time = t + time
                if next_target in target_to_heap_node:
                    node = target_to_heap_node[next_target]
                    if next_time < node[0]:
                        node[2] = True # lazy delete
                        add_target(next_target, next_time)
                else:
                    add_target(next_target, next_time)
            target_to_heap_node.pop(source)
            times[source] = min(times[source], time)
        return max(times.values()) if len(times) == N else -1
```
