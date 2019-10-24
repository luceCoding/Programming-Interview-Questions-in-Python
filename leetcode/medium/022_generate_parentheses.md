# 4. Median of Two Sorted Arrays

## Recursive Solution
- Run-time: Less than O(2^2N)
- Space: 2N
- N = Given N

Any time we deal with different choices, recursion should be the first thing to come to mind.
This problem has a simple decision tree, whether to add a parenthesis or not.
To figure that out, we need a few more items, we need to know the number of open versus closed parenthesis already used to form the result.
Therefore, we are only considering valid parentheses as we recur down the decision tree.

The run-time can be figured out by thinking about number of branches or children each node of tree will have, as well as the depth of the tree.
So you can use the equation O(B^D) for most recursions.
Since there are 2 decisions/branches and a depth of 2N, run-time can be considered O(2^2N).

However, unlike other decision trees, this particular approach is only generating valid parentheses.
So a result like '((((' or '))((' cannot be created if we were to instead traverse the entire decision.
So a run-time of O(2^2N) isn't exactly correct, it is actually faster, again since we are only generating valid parentheses.
It maybe difficult to come up with the actually run-time during an interview but you should at least mention this.

```
class Solution:
    def generateParenthesis(self, n: int) -> List[str]:

        def gen_helper(n_open, n_closed, stack):
            if n_open == n and n_closed == n:
                results.append(''.join(stack))
                return
            if n_open != n:
                stack.append('(')
                gen_helper(n_open + 1, n_closed, stack)
                stack.pop()
            if n_open > n_closed and n_closed != n:
                stack.append(')')
                gen_helper(n_open, n_closed + 1, stack)
                stack.pop()

        results = list()
        gen_helper(0, 0, [])
        return results
```
