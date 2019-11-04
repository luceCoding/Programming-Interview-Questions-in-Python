# 380. Insert Delete GetRandom O(1)

## Solution
- Run-time: O(1)
- Space: O(N)
- N = Number of values

When we insert or delete at O(1), we think of a dictionary.
When we getRandom() at O(1) we think of a randomized index from an array.
If we merge the two, we can achieve O(1).
We just have to do some clever swapping and popping from the last element of the array.

If we use the dictionary to store the value's index in relation to the array.
When we insert, we can insert the new value to the end of the array and keep its value to index relationship in the dictionary.

When it comes to removing the value, we can fetch the value's corresponding index then swap it with the last element in the array.
Then we can just pop that element from the array.
This will help us achieve O(1) run-time.

```
class RandomizedSet:

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.val_to_idx = dict()
        self.values = list()

    def insert(self, val: int) -> bool:
        """
        Inserts a value to the set. Returns true if the set did not already contain the specified element.
        """
        if val in self.val_to_idx:
            return False
        self.values.append(val)
        self.val_to_idx[val] = len(self.values) - 1
        return True


    def remove(self, val: int) -> bool:
        """
        Removes a value from the set. Returns true if the set contained the specified element.
        """
        if val not in self.val_to_idx:
            return False
        idx = self.val_to_idx[val]
        self.val_to_idx[self.values[-1]] = idx
        self.values[idx], self.values[-1] = self.values[-1], self.values[idx]
        del self.val_to_idx[val]
        self.values.pop()
        return True

    def getRandom(self) -> int:
        """
        Get a random element from the set.
        """
        return self.values[random.randrange(0, len(self.values))]
```
