# 518. Coin Change 2

## Sub-Optimal Dynamic Programming Solution
- Runtime: O(A(C^2))
- Space: O(AC)
- C = Number of Coins
- A = Amount

In this first attempt, I will use this example to illustrate the eventual optimal solution.
To understand the solution lets look at this example:

Given coins: [1,2,5]
```
Amounts : Coin Combos : Total Combos 
0: [0] = 1
1: [1] = 1 
2: [1+1, 2] = 2
3: [1+1+1, 2+1] = 2
4: [1+1+1+1, 2+1+1, 2+2] = 3
5: [1+1+1+1+1, 2+1+1+1, 2+2+1, 5] = 4
```

You should notice a pattern here, if we are at amount 5, we can figure out the combinations by using the previous combinations.
So if we are at amount=5 and coin=2, we can take 5-2=3, which is our previous amount.
We find that at amount=3 and coin=2, there is 2+1 as a combination, so we can create 2+1+2 as a new combination.

You can also think of this by two questions, given a previous amount, what are the number of combinations with my coin?
Also, what are the number of combinations without my coin?

Since the question is asking for minimum combinations, we can instead just store the number of combinations per coin for each amount as a dynamic programming 2d array. 
Rows are the amounts and the coins are the columns.

However, you will notice that we have to figure out all the previous combinations for all the coins.
This will eat up our run-time.

```
class Solution:
    def change(self, amount: int, coins: List[int]) -> int:
        coins = list(filter(lambda x: x <= amount, coins))
        n_combos = [[0] * (len(coins)+1) for _ in range(amount+1)]
        n_combos[0][0] = 1
        for a in range(amount+1):
            for c_idx, coin in enumerate(coins, 1):
                if coin == a:
                    n_combos[a][c_idx] += 1
                else:
                    prev_amount = a - coin
                    n_combos[a][c_idx] += sum(n_combos[prev_amount][c] for c in range(0, c_idx+1))
        return sum(n_combos[-1])
```

## Sub-Optimal Dynamic Programming Solution
- Runtime: O(A(C^2))
- Space: O(AC)
- C = Number of Coins
- A = Amount

The previous example was essentially calculating all permutations of the solution.

Instead of building a 2d array, we can just build a 1d array, each element representing number of combinations while each column represents 0 to total amounts.

By condensing each row from the previous solution, we no longer need to sum up all previous combinations.
We can just take all combinations found so far from the last coin to then build upon to the next coin.

So it is important that the loops are in this order, instead of iterating by amounts first, we will iterate by coins.
Remember the idea is to build the next combination from the previous coin, so if we start at coin 1, we will be looking back at coin 0 to see what its combinations were, then we move one to coin 2, coin 2 will be looking back at what coin 1 did, then coin 5 will be looking at coin 2, so on and so forth.

```
class Solution:
    def change(self, amount: int, coins: List[int]) -> int:
        coins = list(filter(lambda x: x <= amount, coins))
        n_combos = [0] * (amount+1)
        n_combos[0] = 1
        for coin in coins:
            for a in range(coin, amount+1):
                n_combos[a] += n_combos[a-coin]
        return n_combos[amount]
```
