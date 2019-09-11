# 322. Coin Change

## Dynamic Programming Bottom Up Solution
- Runtime: O(A) * O(C)
- Space: O(A)
- A = Amount
- C = Number of coins

If we were to start at amount 0 and go up to the given amount.
We can build the minimum coins as we increase the amount by using the previous calculated minimum coins.

For example, if you have an input of [1,5,7] coins.
The minimum coins for amount 1 and 5 is 1. 
When we get to amount 6 with coin 5, we will look at amount 1 (6-5=1) for its minimum coin, which is 1 and do 1+1 at amount 6.
Similarly, when we get to amount 13 with coin 7, we do 13-7=6, look at amount 6 which is 2 minimum coins and do 2+1 for amount 13.
We repeat this until we reach the given amount.

We can do some further optimizations by removing coins that are over the given amount.
We can also use the smallest coin as our starting amount instead of starting at amount 0.

```
class Solution:
    def coinChange(self, coins: List[int], amount: int) -> int:
        coins = list(filter(lambda x: x <= amount, coins)) # remove coins out of range
        amount_to_min_n_coins = [amount+1] * (amount+1)
        amount_to_min_n_coins[0] = 0
        for curr_amount in range(min(coins, default=0), amount+1):
            for coin in coins:
                amount_to_min_n_coins[curr_amount] = min(amount_to_min_n_coins[curr_amount],\
                                                         amount_to_min_n_coins[curr_amount-coin]+1)
        return amount_to_min_n_coins[amount] if amount_to_min_n_coins[amount] <= amount else -1
```

```
import sys

class Solution:
    def coinChange(self, coins: List[int], amount: int) -> int:
        min_amounts = [-1] * (amount+1)
        min_amounts[0] = 0
        for a in range(1, amount+1):
            curr_min = sys.maxsize
            for coin in coins:
                prev_min = a - coin
                if prev_min >= 0 and min_amounts[prev_min] != -1:
                    curr_min = min(curr_min, min_amounts[prev_min]+1)
            min_amounts[a] = curr_min if curr_min != sys.maxsize else -1
        return min_amounts[amount]
```
