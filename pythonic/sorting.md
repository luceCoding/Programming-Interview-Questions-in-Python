This section, I'll be going over the best ways to sort in Python.

## Sorting a list of integers

sorted() creates a new list after the sort.
```
result = [3,4,2,1,5]
sorted(result)
>>> [1,2,3,4,5]
```

.sort() sorts the exisiting list in-place, therefore modifying the list.
```
result = [3,4,2,1,5]
result.sort()
print(result)
>>> [1,2,3,4,5]
```

To sort in reverse, pass in the reverse keyword as True.
```
sorted(result, reverse=True)
```
```
result.sort(reverse=True)
```

## Sorting a list of tuples
```
result = [(1,3),(1,2),(2,3),(2,1),(1,1)]
result.sort()
```

Sorting a list of tuples based on a specific value per tuple

```
result.sort(key=lambda x: x[1])
```

Sorting a list of tuples with tiebreaker.

```
result.sort(key=lambda x: (x[0], x[1]))
```

Sorting a list of tuples with tiebreaker but second value is sorted in reversed.
```
result.sort(key=lambda x: (x[0], -x[1]))
>>> [(1, 3), (1, 2), (1, 1), (2, 3), (2, 1)]
```
## Sorting a list of tuples which contain strings

## Sorting based on a custom sort
