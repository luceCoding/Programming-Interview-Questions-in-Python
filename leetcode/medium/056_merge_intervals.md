# 56. Merge Intervals

## Solution
- Runtime: O(Nlog(N))
- Space: O(1) (Assuming result isn't considered extra space)
- N = Number of elements in intervals

By sorting the intervals, we can then recreate the result by comparing previous intervals with the current interval from left to right.
In this way, we can check if the two intervals overlap each other after the sort.
We can keep a temporary new interval that we compare to as we loop.
When finding a non-overlapping interval, we can insert the new interval into the result and set the new interval as the current interval.

```
class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        if len(intervals) == 0:
            return []
        intervals.sort(key=lambda x: x[0])
        result = list()
        new_interval = intervals[0]
        for interval in intervals[1:]:
            if interval[0] <= new_interval[1]: # do they overlap?
                new_interval[1] = max(interval[1], new_interval[1])
            else:
                result.append(new_interval)
                new_interval = interval
        result.append(new_interval)
        return result
```
