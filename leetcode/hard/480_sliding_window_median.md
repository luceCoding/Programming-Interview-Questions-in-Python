# 480. Sliding Window Median

## Sort Solution
- Run-time: O(N*K)
- Space: O(K)
- N = Number of elements in nums
- K = Given k value

Similar to question 295.

By keeping a sorted array of size K, we can use binary search for each new number at O(logK) run-time.
However, due to the requirement of the sliding window, we need to find the previous value that is outside of the sliding window and remove it from the sorted array. This takes O(logK) with binary search to find but O(K) to rebuild the array after deletion.
We would then have to do this N times, therefore O(N*K) overall run-time.

```
import bisect

class Solution:
    def medianSlidingWindow(self, nums: List[int], k: int) -> List[float]:
        window, results = list(), list()
        median_idx = k // 2
        for idx, n in enumerate(nums):
            bisect.insort(window, n)
            if len(window) > k:
                window.pop(bisect.bisect_left(window, nums[idx-k]))
            if len(window) == k:
                results.append(window[median_idx] if k % 2 \
                    else (window[median_idx-1] + window[median_idx]) / 2)
        return results
```

Slightly better performance but same big O run-time.
```
import bisect

class Solution:
    def medianSlidingWindow(self, nums: List[int], k: int) -> List[float]:
        window, results = list(nums[0:k-1]), list()
        window.sort()
        median_idx = k // 2
        for idx, n in enumerate(nums[k-1:], k-1):
            bisect.insort(window, n)
            results.append(window[median_idx] if k % 2 \
                else (window[median_idx-1] + window[median_idx]) / 2)
            window.pop(bisect.bisect_left(window, nums[idx-k+1]))
        return results
```
