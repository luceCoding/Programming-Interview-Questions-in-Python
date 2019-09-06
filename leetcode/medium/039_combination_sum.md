# 39. Combination Sum

## DFS Recursion Solution

- Runtime: TBD
- Space: O(T)
- N = Number of elements in array
- T = Target number

```
class Solution:
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        
        def dfs(curr_idx, curr_sum, stack, results):
            if curr_sum > target:
                return
            elif curr_sum == target:
                results.append(copy.deepcopy(stack))
                return
            for idx in range(curr_idx, len(candidates)):
                stack.append(candidates[idx])
                dfs(idx, curr_sum+candidates[idx], stack, results)
                stack.pop()
        
        results = list()
        candidates.sort()
        dfs(0, 0, [], results)
        return results
```
