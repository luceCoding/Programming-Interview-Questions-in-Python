# 123. Best Time to Buy and Sell Stock III

## Dynamic Programming Solution
- Runtime: O(N)
- Space: O(N)
- N = Number of elements in array

By using divide in conquer, we can figure out the correct times to buy twice.
If you did the question, Best Time to Buy and Sell Stock I, you may apply that algothrim here.

The idea is to find the best transaction to the left of the price and again to the right of the price.
We can use two arrays and traverse from left to right then again from right to left to find those transactions.
Then its a matter of traversing a third time but on the two new arrays to find the two best transactions.

```
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        if len(prices) == 0:
            return 0
        left_dp, right_dp = [0] * len(prices), [0] * len(prices)
        
        high = low = prices[0]
        left_max_profit = 0
        for index, price in enumerate(prices): # left to right
            if price < low:
                high = low = price
            else:
                high = max(high, price)
            left_dp[index] = left_max_profit = max(high - low, left_max_profit)
            
        high = low = prices[-1]
        right_max_profit = 0
        for index, price in enumerate(prices[::-1]): # right to left
            index = len(prices)-index-1
            if price > high:
                high = low = price
            else:
                low = min(low, price)
            right_dp[index] = right_max_profit = max(high - low, right_max_profit)

        return max([left_dp[i]+right_dp[i] for i in range(len(prices))])
```
