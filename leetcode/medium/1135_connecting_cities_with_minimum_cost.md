# 1135. Connecting Cities With Minimum Cost

## Prim's Algorithm Solution with Heaps
- Run-time: O((V + E)logV)
- Space: O(V)
- V = Vertices
- E = Edges

This question requires a minimum spanning tree (MST) algorithm.
I choose to use Prim's algorithm over Kruskal's algorithm because it is similar to Dijkstra's algorithm, both are greedy algorithms.
Knowing one will help you learn the other.

The pseudocode could be represented as such:
1. Select an arbitrary vertex to start with and add it to the heap.
2. While the heap isn't empty:
    - Select the min edge/weight of an unvisited vertex from top of heap.
    - Add the selected vertex to the visited vertices.
    - Take note of the cost or path taken.
    - Add/Update selected vertex's neighboring edges/weights of unvisited vertices into the heap.

As of the time of writing this, Python 3.7 doesn't have an adaptable priority queue implementation.
If there was a real adaptable priority queue, there would be a feature to update a specific city's cost in the heap and bubble up and down the heap to maintain the heap property.
To read more about adaptable priority queues, check out [Data Structures and Algorithms in Python 1st Edition](https://www.amazon.com/Structures-Algorithms-Python-Michael-Goodrich/dp/1118290275).

This example is more of a lazy heap implementation, looking at the heapq documentation, they show an implementation of a lazy adaptable priority queue.
This will help us get O((V + E)logV) run-time.
Without performing the lazy delete technique with a heaq, we would get O((V + E)^2logV) run-time.
That is because in a undirected dense graph, we would be adding duplicate entries of the same nodes into the heap.
There are a lot of incorrect implementations of Prim's Algorithm using heapq that do not perform lazy deletes out there, so be warned.

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

        def add_city(new_city, new_cost):
            new_node = [new_cost, new_city, False]
            city_to_heap_node[new_city] = new_node
            heapq.heappush(min_heap, new_node)

        adj_list = create_adj_list()
        start_node = [0, 1, False] # cost, city, remove
        min_heap = list([start_node]) # start at city #1
        city_to_heap_node = dict({1 : start_node})
        visited, min_cost = set(), 0
        while min_heap:
            cost, city, remove = heapq.heappop(min_heap)
            if remove: # lazy delete
                continue
            visited.add(city)
            min_cost += cost
            for next_city, next_cost in adj_list[city]:
                if next_city in visited:
                    continue
                elif next_city in city_to_heap_node:
                    heap_node = city_to_heap_node[next_city]
                    if next_cost < heap_node[0]: # lower cost?
                        heap_node[2] = True # lazy delete
                        add_city(next_city, next_cost)
                else:
                    add_city(next_city, next_cost)
            city_to_heap_node.pop(city)
        return min_cost if len(visited) == N else -1
```
