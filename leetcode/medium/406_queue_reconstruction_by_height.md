# 406. Queue Reconstruction by Height

## Best Solution
- Runtime: O(Nlog(N))
- Space: O(1) (Assuming the result isn't extra space)
- N = Number of elements in array

There will be two sorts involved. One sorts the array by height, followed by an insertion sort by index.

1. Sort the people by their height, highest to shortest, if heights are same, sort by index, smallest to largest.
2. Starting with the highest people, insert them into the result via. their indexes.

The logic behind this is by starting with the highest people, we guarantee that the next element inserted via. their index, will be inserted in the correct slot compared to their counter parts in respect to their index.
```
Try inserting: [[7,0], [6,0], [5,0]] -> [[5,0], [6,0], [7,0]]
Then try: [[7,0], [6,0], [5,0], [4,1]] -> [[5,0], [4,1], [6,0], [7,0]]
```

```
Input: [[7,0], [4,4], [7,1], [5,0], [6,1], [5,2]]

Sorted: [[7,0], [7,1], [6,1], [5,0], [5,2], [4,4]]

Insertion:
1. [[7,0]]
2. [[7,0], [7,1]]
3. [[7,0], [6,1], [7,1]]
4. [[5,0], [7,0], [6,1], [7,1]]
5. [[5,0], [7,0], [5,2], [6,1], [7,1]]
6. [[5,0], [7,0], [5,2], [6,1], [4,4], [7,1]]
```

The first sort is O(Nlog(N)) while the second sort is O(N) due to known indexes for insertion.

```
class Solution:
    def reconstructQueue(self, people: List[List[int]]) -> List[List[int]]:
        people.sort(key=lambda x: (x[0], -x[1]), reverse=True)
        result = list()
        for p in people:
            if p[1] >= len(result):
                result.insert(len(result), p)
            else:
                result.insert(p[1], p)
        return result
```
