# 937. Reorder Data in Log Files

## Sorted In Place Solution
- Runtime: O(Nlog(N))
- Space: O(1)
- N = Number of logs

This is a common Amazon question asked during online assessment tests.

```
class Solution:
    def reorderLogFiles(self, logs: List[str]) -> List[str]:
        def getKey(log):
            identifier, words = log.split(' ', 1)
            if words[0].isdigit():
                return (1, 0, 0)
            else:
                return (0, words, identifier)
        
        logs.sort(key=getKey)
        return logs
```
