# 322. Coin Change

## Dynamic Programming Bottom Up Solution
- Runtime: O(A) * O(C)
- Space: O(A)
- A = Amount
- C = Number of coins

If we were to start at amount 0 and go up to the given amount.
We can build the minimum coins as we increasing the amount by using the previous calculated minimum coins.

For example, if you have an input of [1,5,6] coins.
As we known the minimum coins for amount 1 and 5 is 1. 
When we get to amount 6 with coin 5, we will look at amount 1 (6-5=1) for its minimum coin, which is 1 and do 1+1 at amount 6.
Similarly, when we get to amount 12 with coin 6, we do 12-6=6, look at amount 6 which is 2 minimum coins and do 2+1 for amount 12.
We repeat this until we reach the given amount.

We can do some further optimizations by removing coins that are over the given amount.
We can also use the minimum coin as our starting amount instead of starting at amount 0.

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
