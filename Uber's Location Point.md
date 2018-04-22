# QUESTION
A rule looks like this:
```
A NE B
```
This means this means point A is located northeast of point B.
```
A SW C
```
means that point A is southwest of C.

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
            graph[node1].append(node2)
    else:
        graph[node1] = list()
        graph[node1].append(node2)
        
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
            return find_cycle(neighbor, graph, visited)
    return False
    
test_strings1 = ['A NW B', 
                 'A N B']
    
test_strings2 = ['A N B', 
                'B NE C',
                'C N A']

assert are_locations_valid(test_strings1) == True
assert are_locations_valid(test_strings2) == False
```
