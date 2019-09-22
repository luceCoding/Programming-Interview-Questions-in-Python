# 49. Group Anagrams

## Sort Solution

- Runtime: O(NSlog(S))
- Space: O(NS)
- N = Number of strings in list
- S = Longest string

We can use a dictionary to group anagrams together.
However, to figure out wherther two strings should be grouped together, we can create the same key by sorting them.

```
from collections import defaultdict

class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        grouped_anagrams = defaultdict(list)
        for word in strs:
            key = ''.join(sorted(word))
            grouped_anagrams[key].append(word)
        return grouped_anagrams.values()
```

## Hash Solution

- Runtime: O(NS)
- Space: O(NS)
- N = Number of strings in list
- S = Longest string

We can improve upon the previous solution by figuring out a better way to create the exact same key to group by.
We will have to create a good hash function.
Since we know the actually ranges, that is 26 letters, we can just create an array of size 26 and place the counts of each letter for each of the 26 buckets.
We have to create a tuple version of this list because you cannot hash a mutable object in Python, only immutable objects.

It is also very important to note that there MUST be some sort of delimiter inbetween each count/bucket of the hash code.
Else you can have an input where the hash code is a bunch of 1s but you can't tell which character has a count of 1, 11 or 111 etc...
Hence, creating a bad hash code.

```
from collections import defaultdict

class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        def get_hash(word):
            buckets = [0] * 26
            for ch in word:
                idx = ord(ch) - ord('a')
                buckets[idx] += 1
            return (tuple(buckets))
        
        grouped_anagrams = defaultdict(list)
        for word in strs:
            key = get_hash(word)
            grouped_anagrams[key].append(word)
        return grouped_anagrams.values()
```
