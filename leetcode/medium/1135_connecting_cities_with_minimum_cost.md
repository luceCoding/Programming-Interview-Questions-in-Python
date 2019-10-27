# 1135. Connecting Cities With Minimum Cost

## Prim's Algorithm Solution
- Run-time: O(N^2)
- Space: O(N)
- N = Number of cities

This is question requires a minimum spanning tree (MST) algorithm.
I choose to use Prim's algorithm because it is similar to Dijkstra's algorithm over Kruskal's algorithm, both are greedy algorithms.
Knowing one will help you in learning the other.

The idea with Prim's is to utilize a heap of vertices and an adjacent list.
The resulting MST cannot produce a cycle due to the method of selecting an edge of an unvisited vertex.
To select an edge, we use the heap sorted by the cost or weight of the edge.
Each time we pop from the heap, we mark that vertex as visited and add it's neighboring vertices' edges to the heap.

The pseudocode could be represented as such:
1. Select an arbitrary vertex to start with and add its neighboring vertices to the heap.
2. Mark that arbitrary vertex as visited.
3. While there are unvisited vertices:
    - Select the edge with the min weight of an unvisited vertex from top of heap.
    - Add the selected vertex to the visited vertices.
    - Add selected vertex's neighboring edges of unvisited vertices into the heap.

As of the time of writing this, Python 3.7 doesn't have an adaptable priority queue implementation.
So this is why this version of Prim's runs at O(N^2) time.
This example is more of a lazy heap implementation.
If we were to use an adaptable priority queue, we would see O(MlogN) run-time.
This is because we will only have at most N cities in the heap, no duplicates.
There would be a feature to update a specific city's cost in the heap and bubble up and down the heap to maintain the heap property.
With an adaptable priority queue, there would be no need to use a visited set.

To read more about adaptable priority queues, check out [Data Structures and Algorithms in Python 1st Edition](https://www.amazon.com/Structures-Algorithms-Python-Michael-Goodrich/dp/1118290275).

```
from collections import defaultdict

class Solution:
    def minimumCost(self, N: int, connections: List[List[int]]) -> int:

        def create_adj_list():
            adj_list = defaultdict(list)
            for n in range(1, N + 1): # incase there are cities with no connections
                adj_list[n]
            for city1, city2, cost in connections:
                adj_list[city1].append((city2, cost))
                adj_list[city2].append((city1, cost))
            return adj_list

        adj_list = create_adj_list()
        min_heap = list([(0, 1)]) # start at city #1
        visited = set()
        min_cost = 0
        while min_heap and len(visited) != N:
            cost, city = heapq.heappop(min_heap)
            if city in visited: # skip this city
                continue
            visited.add(city)
            min_cost += cost
            for next_city, next_cost in adj_list[city]:
                if next_city in visited:
                    continue
                heapq.heappush(min_heap, (next_cost, next_city))
        return min_cost if len(visited) == N else -1
```
