# 78. Subsets

## Recursive Solution
- Runtime: O(2^N)
- Space: O(N)
- N = Number of elements in array

The intuition is recognize that for each number, we can either add it or not.
Therefore, using recursion, we can easily backtrack the solution and try choice #2.

Using the ability for recursion to backtrack will allow us to populate the result.
During each recursion, we will loop through the given array, during this loop, the number represent a choosen number for the subset.
The numbers that were not choosen yet will be passed to the next recursion to be choosen again, hence, creating the result.
By keeping a stack, we can use it as we traverse/recur, we append prior and pop after using this stack to add to the result of subsets.

You can visually map this out using this example:

Input: [1,2,3]

1. R1: Select 1 -> [1]
2. R2: Select 2 -> [1,2]
3. R3: Select 3 -> [1,2,3] -> Pop 3 -> Done
5. R2: Pop 2 -> Select 3 -> [1,3] -> Pop 3 -> Done
7. R1: Pop 1 -> Select 2 -> [2]
8. R2: Select 3 -> [2,3] -> Pop 3 -> Done
9. R1: Select 3 -> [3] -> Pop 3 -> Done

```
class Solution(object):
    def subsets(self, nums):
        
        def subset_helper(idx, results, path=[]):
            if idx == len(nums):
                results.append(path)
                return
            # add
            subset_helper(idx+1, results, path+[nums[idx]])
            # don't add
            subset_helper(idx+1, results, path)
            
        results = list()
        subset_helper(0, results)
        return results
```
