# SOLUTION (Inefficient Solution)
When you see a question like this It is best to try this out on the first 3-4 Ns on paper. You want to recognize a pattern that you can continue to do over and over again. Once that pattern is found, you can think of a recursive solution from top-down or bottom-up.

This solution does a bottom up approach of building the result from 0 to N. 
Using the previously calculated N, it will append a '()' to the front of it and save it for the current N. It will also append a '()' after every '(' found.

However, during the build phase, it can create duplicate results. You would notice this duplication if you tried it on paper first.
```
class Solution(object):
    def generateParenthesis(self, n):
        """
        :type n: int
        :rtype: List[str]
        """
        # we need a list of sets
        # this solution creates duplicates of the same results
        prev_parenthesis = list()
        prev_parenthesis.append(set(['']))
        self.gen_parenthesis_helper(1, n, prev_parenthesis)
        return list(prev_parenthesis[n])
    
    def gen_parenthesis_helper(self, curr_n, n, prev_parenthesis):
        if curr_n > n:
            return
        prev_parenthesis.append(set())
        prev_parenthesises_set = prev_parenthesis[curr_n-1]
        # using the prev parenthesis
        # for each element add a '()' in the front and for every open bracket found
        for parenthesis in prev_parenthesises_set:
            new_parenthesis = '()' + parenthesis
            prev_parenthesis[curr_n].add(new_parenthesis)
            for index, ch in enumerate(parenthesis):
                if ch == '(':
                    new_parenthesis = parenthesis[0:index+1] + '()' + parenthesis[index+1:]
                    prev_parenthesis[curr_n].add(new_parenthesis)
        self.gen_parenthesis_helper(curr_n+1, n, prev_parenthesis)
```

# Solution (Efficient Solution)
```

```
