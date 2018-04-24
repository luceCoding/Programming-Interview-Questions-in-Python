# QUESTION
A rule looks like this:
```
A N B
```
This means point A is somewhere north of point B. It could be northeast, northwest or north. You arn't sure.

```
A NE B
```
This means this means point A is located exactly northeast of point B.
```
A SW C
```
means that point A is exactly southwest of C.

Given a list of rules, check if the sum of the rules validate. For example:
```
A N B
B NE C
C N A
```
does not validate, since A cannot be both north and south of C.

```
A NW B
A N B
```
is considered valid.

# EXPLAINATION
This is question is a graph problem. It is easily recongized when you notice that A and B are creating relationships with each other. Now, if you were to tackle this problem the traditional way, that is, create an entire adjacency matrix or list with north, south, east, west relationships. Your code would be very complex.

Instead, think about how the non-valid case is found. You will notice that you must figure out if the graph is acyclic. That is, if B points to A then if A points back to B, you have a non-valid direction. You can solve this by creating a directed graph.

The next issue is, which node should you begin with to start finding acyclic paths? Ideally, for a non-cyclic graph, starting at the node that doesn't have any nodes pointing to it is ideal. But we can never know which one that would be for sure. If we were to search for cycles for each node in the graph, our run-time complexity increases greatly to N^N, N being number of total nodes.

Instead, we must use four directed graphs for north, south, east and west to deal with this issue. We can pick an arbitary node, say node A, for north and south, then another arbitary node, say node B, for east and west. Then with node A, we search north then south for cyclic relationships. We then do the same for node B for east and west.

Run-time: O(n)

Space: O(n)

n = number of nodes

# SOLUTION
```
def are_locations_valid(strings):
    north_directed_graph = dict()
    south_directed_graph = dict()
    east_directed_graph = dict()
    west_directed_graph = dict()
    NS_starter_node, EW_starter_node = '', ''
    for string in strings:
        tokens = string.split(' ')
        node1, directions, node2 = tokens
        if 'N' in directions or 'S' in directions:
            add_to_graph(north_directed_graph, node1, node2)
            add_to_graph(south_directed_graph, node2, node1)
            NS_starter_node = node1
        if 'E' in directions or 'W' in directions:
            add_to_graph(east_directed_graph, node1, node2)
            add_to_graph(west_directed_graph, node2, node1)
            EW_starter_node = node1
    return does_cycle_exist(north_directed_graph, NS_starter_node) == False \
            and does_cycle_exist(south_directed_graph, NS_starter_node) == False \
            and does_cycle_exist(east_directed_graph, EW_starter_node) == False \
            and does_cycle_exist(west_directed_graph, EW_starter_node) == False

def add_to_graph(graph, node1, node2):
    if node1 in graph:
        if node2 not in graph[node1]:
            graph[node1].add(node2)
    else:
        graph[node1] = set()
        graph[node1].add(node2)
        
def does_cycle_exist(graph, starter_node):
    if starter_node in graph:
        visited = set()
        return find_cycle(starter_node, graph, visited)
    return False

def find_cycle(node, graph, visited):
    if node in visited:
        return True
    visited.add(node)
    if node in graph:
        for neighbor in graph[node]:
            if find_cycle(neighbor, graph, visited):
                return True
    return False
    
test_strings1 = ['A NW B', 
                 'A N B']

assert are_locations_valid(test_strings1) == True

test_strings2 = ['A N B', 
                'B NE C',
                'C N A']

assert are_locations_valid(test_strings2) == False

test_strings3 = ['A N B',
                 'A N C',
                 'C N B']

assert are_locations_valid(test_strings3) == True

test_strings4 = ['A N B',
                 'A N C',
                 'B N A',
                 'C N B']

assert are_locations_valid(test_strings4) == False
```
