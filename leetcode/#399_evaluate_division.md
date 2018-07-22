# EXPLAINATION
Once the mathy part of the question. I know its a bit tricky to understand how a/b=2.0, b/c=3.0 which means a/c=6.0. 
Just keep in mind that you can multiply previous calculated answers. By looking at the examples, you can easily find that relationship.

So it should be very obvious this is a graph problem. However, unlike other graph problems, edges have weights associated to them. You need to keep track of them when building your graph.
You can use DFS to help here. It is also important to ask the interviewer whether there is a scenario where there is more than one path to the same node and whether that result would change.
For example, a->b->c is a path but also there is a->d->e->c. Just to make sure if there is a distinction between a longer/shorter path(use BFS) and whether there will be more than one result.
For this question, lets not care about that scenario.

Another edge case to consider is a cycle happening in itself. Also, by knowing a->b is 2.0 then what is b->a? In math, its just the inverse(0.5).

# SOLUTION
```
class Solution(object):
    def calcEquation(self, equations, values, queries):
        """
        :type equations: List[List[str]]
        :type values: List[float]
        :type queries: List[List[str]]
        :rtype: List[float]
        """
        adj_list = self.create_adj_list(equations, values)
        results = list()
        for from_node, to_node in queries:
            local_visit_set = set()
            calc_stack = list()
            local_result = self.dfs_get_query(from_node, to_node, calc_stack, local_visit_set, adj_list)
            results.append(local_result)
        return results
    
    def dfs_get_query(self, curr_node, target_node, calc_stack, local_visit_set, adj_list):
        if curr_node not in adj_list or curr_node in local_visit_set:
            return -1.0
        if curr_node == target_node:
            return self.calc(calc_stack)
        local_visit_set.add(curr_node)
        next_nodes = adj_list[curr_node]
        for next_node, weight in next_nodes:
            calc_stack.append(weight)
            local_result = self.dfs_get_query(next_node, target_node, calc_stack, local_visit_set, adj_list)
            calc_stack.pop()
            if local_result != -1:
                return local_result
        local_visit_set.remove(curr_node)
        return -1.0
    
    def create_adj_list(self, equations, values):
        result = dict()
        curr_index = 0
        while curr_index < len(equations):
            from_node, to_node = equations[curr_index]
            value = values[curr_index]
            
            if from_node not in result:
                result[from_node] = list()
            new_node = (to_node, value)
            result[from_node].append(new_node)
            
            if to_node not in result:
                result[to_node] = list()
            new_node = (from_node, 1.0/value)
            result[to_node].append(new_node)
            
            curr_index += 1
        return result
    
    def calc(self, stack):
        result = 1.0
        for element in stack:
            result *= element
        return result
```
