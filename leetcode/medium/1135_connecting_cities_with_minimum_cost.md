# 1135. Connecting Cities With Minimum Cost

## Prim's Algorithm Solution
- Run-time: O((V + E) log(V))
- Space: O(E)
- E - Number of Edges

This is question requires a minimum span tree(MST) algothrim.
I choose to use Prim's algothrim because it is similar to Dijkstra's algorithm over Kruskal's algorithm.
Knowing one will help you in learning the other.

The idea with Prim's is to utilize a heap of vertices and an adjacent list.
The resulting MST cannot produce a cycle due to the method of selecting an edge of an unvisited vertex.
To select an edge, we use the heap sorted by the cost or weight of the edge.
Each time we pop from the heap, we mark that vetrex as visited and add it's neighboring vertices' edges to the heap.

The pseudocode could be represented as such:
1. Select an arbitrary vertex to start with and add its neighboring vertices to the heap.
2. Mark that arbitrary vertex as visited.
3. While there are unvisited vertices:
    - Select the edge with the min weight of an unvisited vertex from top of heap.
    - Add the selected vertex to the visited vertices.
    - Add selected vertex's neighboring edges of unvisited vertices into the heap.
    
Since we are sorting based on vertices, the sorting will take log(V) run-time.
However, we have to sort this based on number of vertices or edges because we can be given a graph of islands or nodes that have edges to all nodes.
That is why its O((V+E) log(V)) run-time.

```
from collections import defaultdict

class Solution:
    def minimumCost(self, N: int, connections: List[List[int]]) -> int:
        
        def create_adj_list(connections):
            adj_list = defaultdict(list)
            for city1, city2, cost in connections:
                adj_list[city1].append(Vertex(city2, cost))
                adj_list[city2].append(Vertex(city1, cost))
            return adj_list
                
        adj_list = create_adj_list(connections)
        visited = set([N])
        min_heap = list(adj_list[N] if N in adj_list else [])
        heapq.heapify(min_heap)
        min_cost = 0
        while min_heap and len(visited) != N:
            vertex = heapq.heappop(min_heap)
            if vertex.to in visited:
                continue
            visited.add(vertex.to)
            min_cost += vertex.cost
            for vertex in adj_list[vertex.to]:
                heapq.heappush(min_heap, vertex)
        return min_cost if len(visited) == N else -1
    
class Vertex(object):
    
    def __init__(self, to, cost):
        self.to = to
        self.cost = cost
        
    def __lt__(self, other):
        return self.cost < other.cost
    
    def __repr__(self):
        return 'To: {}, Cost: {}'.format(self.to, self.cost)
```
