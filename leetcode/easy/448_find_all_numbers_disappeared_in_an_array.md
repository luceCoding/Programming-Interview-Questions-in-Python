# 448. Find All Numbers Disappeared in an Array

## Solution with set
- Runtime: O(N)
- Space: O(N)
- N = Number of elements in array

Simply iterate through the list and save the numbers into a set. 
Then iterate a second time with the numbers from 1 to N to find missing numbers in the set.

```
class Solution:
    def findDisappearedNumbers(self, nums: List[int]) -> List[int]:
        seen = set()
        result = list()
        for n in nums:
            seen.add(n)
        for num in range(1, len(nums)+1):
            if num not in seen:
                result.append(num)
        return result
```

## Best Solution
- Runtime: O(N)
- Space: O(1)
- N = Number of elements in array

Similar to the set approach, we can reuse the original list as our set.
Inorder to achieve this, we will have to use the fact that integers are between 1 and the size of the array.
Hence, there is a one to one relationship between the number and its index.

We will iterate through the list once and for a given number, go to its index that its suppose to be at and negate whatever number is there.
The negation will tell us that we found a home for a number at that location. 
Then we will iterate the list a second time and find all the numbers that were not negated, take its index and we will then know the missing number.

```
class Solution:
    def findDisappearedNumbers(self, nums: List[int]) -> List[int]:
        results = list()
        for n in nums:
            other_index = abs(n)-1
            nums[other_index] = -abs(nums[other_index])
        return [index+1 for index in range(0, len(nums)) if nums[index] > 0]
```
