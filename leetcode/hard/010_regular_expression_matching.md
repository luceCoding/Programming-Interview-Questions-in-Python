# 10. Regular Expression Matching

## Bottom-Up Dynamic Programming Solution
- Runtime: O(P*S)
- Space: O(P*S)
- P = Number of letters in pattern
- S = Number of letters in string

By using a bottom up dynamic programming solution, we attempt to build the solution from previously calculated pattern and string combinations.

If given S = 'ba' and P = 'b.' we can draw a grid as such:
```
   '' b a
''  T F F
b   F T F
.   F F T
```
Notice we start with an empty string and pattern, this is always True. Then we go left to right starting with the first row.
Each row is the current pattern letter and each column is the current string letter.

1. S = 'b', P = 'b' -> True
2. S = 'ba', P = 'b' => False
3. Then we move on to the next pattern letter.
4. S = 'b', P = 'b.' -> False
5. S = 'ba', P = 'b.' -> True

Notice that as long as the character match or the pattern == '.' we then care that the previous string and pattern is true.
That is equivalent to the top-left of each element in the dp array.

Now the wildcard is the trickest part of the question.
In respect to the wildcard, we will need to break this down into three parts.
1. Can I occur only once?
2. Can I occur zero times?
3. Can I occur multiple times?

In using the dynamic programming array, what do we need from it to match these three cases?
In respect to the wildcard pattern character:
1. Occuring only once can be achieved by looking at the previous pattern letter on the same string letter, basically the one above the current dp element. You will also need to check if the character before the wildcard matches your current string letter. If given P = 'ba*' and S = 'ba', you basically want to check if previous pattern P = 'ba' at S = 'ba' was True.
2. For occuring zero times, we need to look at the previous 2 pattern letters if thats true. If given P = 'ba*' and S = 'b', you basically want to check if previous pattern P = 'b' at S = 'b' was True. 
3. Occurring multiple times is the trickest of the three. You need to make sure the previous pattern letter matches your current string letter or if the pattern = '.' while the dp value to the left is true. Given P = 'ba*' and S = 'baaa', we want to know if P = 'ba*' at S = 'ba' was true, then if P = 'ba*' at S = 'baa' was True.

We will also need to intialize our dynamic programming array correctly.
The first column is matching aganist S = '' an empty string.
So if you have a pattern P = 'b\*a\*', all wildcards postions are True because they can occur zero times.

```
class Solution:
    def isMatch(self, string: str, pattern: str) -> bool:
        dp = [[False] * (len(string)+1) for _ in range(len(pattern)+1)]
        self.intialize(dp, pattern)
        for i_p, p in enumerate(pattern):
            for i_s, s in enumerate(string):
                if (p == s or p == '.') and dp[i_p][i_s]:
                    dp[i_p+1][i_s+1] = True
                elif p == '*':
                    if dp[i_p][i_s+1] and i_p > 0 and pattern[i_p-1] == s: # can I occur once?
                        dp[i_p+1][i_s+1] = True
                    elif i_p > 0 and dp[i_p-1][i_s+1]: # can I occur zero times?
                        dp[i_p+1][i_s+1] = True
                    elif i_p > 0 and (pattern[i_p-1] == s or pattern[i_p-1] == '.') and dp[i_p+1][i_s]: # can I occur multiple times?
                        dp[i_p+1][i_s+1] = True
        return dp[-1][-1]
    
    def intialize(self, dp, pattern):
        dp[0][0] = True
        for index, p in enumerate(pattern):
            # can I occur zero times or occur once (p=* and s='')??
            if p == '*' and (dp[index][0] or (index > 0 and dp[index-1][0])):
                dp[index+1][0] = True
```
