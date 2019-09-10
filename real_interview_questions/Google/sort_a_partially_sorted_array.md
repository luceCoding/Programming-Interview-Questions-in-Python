# Question
Given an array of positive integers (possibly with duplicates) such that the numbers have been sorted only by 28 most significant bits. Sort the array completely.

Example 1:

Input: [0, 15, 12, 17, 18, 19, 33, 32]
Output: [0, 12, 15, 17, 18, 19, 32, 33]

```
Explanation:
The integers in their binary representation are:
 0 = 0000 0000 0000 0000 0000 0000 0000 0000
15 = 0000 0000 0000 0000 0000 0000 0000 1111
12 = 0000 0000 0000 0000 0000 0000 0000 1100
17 = 0000 0000 0000 0000 0000 0000 0001 0001
18 = 0000 0000 0000 0000 0000 0000 0001 0010
19 = 0000 0000 0000 0000 0000 0000 0001 0011
33 = 0000 0000 0000 0000 0000 0000 0010 0001
32 = 0000 0000 0000 0000 0000 0000 0010 0000

In sorted order:
 0 = 0000 0000 0000 0000 0000 0000 0000 0000
12 = 0000 0000 0000 0000 0000 0000 0000 1100
15 = 0000 0000 0000 0000 0000 0000 0000 1111
17 = 0000 0000 0000 0000 0000 0000 0001 0001
18 = 0000 0000 0000 0000 0000 0000 0001 0010
19 = 0000 0000 0000 0000 0000 0000 0001 0011
32 = 0000 0000 0000 0000 0000 0000 0010 0000
33 = 0000 0000 0000 0000 0000 0000 0010 0001
```

```
Example 2:

Input: [100207, 100205, 100204, 100206, 100203]
Output: [100203, 100204, 100205, 100206, 100207]
Explanation:
The integers in their binary representation are:
100207 = 0000 0000 0000 0001 1000 0111 0110 1111
100205 = 0000 0000 0000 0001 1000 0111 0110 1101
100204 = 0000 0000 0000 0001 1000 0111 0110 1100
100206 = 0000 0000 0000 0001 1000 0111 0110 1110
100203 = 0000 0000 0000 0001 1000 0111 0110 1011

In sorted order:
100203 = 0000 0000 0000 0001 1000 0111 0110 1011
100204 = 0000 0000 0000 0001 1000 0111 0110 1100
100205 = 0000 0000 0000 0001 1000 0111 0110 1101
100206 = 0000 0000 0000 0001 1000 0111 0110 1110
100207 = 0000 0000 0000 0001 1000 0111 0110 1111
```

Expected O(n) time solution.

## Solution

- Runtime: O(N)
- Space: O(1), assuming result doesn't use space
- N = Number of elements in array

The idea is to use bucket sort, since the array is already partially sorted by the first 28 bits.
During this time, we will place the numbers into 1 of 16 buckets based on the last 4 bits.
We can just iterate from left to right until the first 28 bits are different.
Then we can save the results we have in the buckets and reset.
With this approach, we will never need more than 16 buckets at a given time.

```
def sort_partial_sorted_28b(nums):
    if len(nums) == 0:
        return nums
    mask = ~0 << 4
    curr_28b = nums[0] & mask 
    buckets = [0] * 16 # 16 buckets -> n occurances
    results = list()
    for num in nums:
        if (num & mask) != curr_28b: # start sort
            for bucket, occurance in enumerate(buckets):
                for _ in range(occurance):
                    results.append(curr_28b | bucket)
            curr_28b = num & mask # set to next 28 bit group
            buckets = [0] * 16 # reset
        # add to buckets
        buckets[num & 15] += 1
    for bucket, occurance in enumerate(buckets):
        for _ in range(occurance):
            results.append(curr_28b | bucket)
    return results
```
