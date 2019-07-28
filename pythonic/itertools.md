# Itertools
This section will go over a few tools in python 3's itertools library.

## count()

## cycle()

## repeat()

## accumulate()

## chain()

## compress()

## groupby()
Requires that the iterable is sorted by the value you are grouping by.

## dropwhile()

## islice()

## starmap()
Used for when you have an iterable of iterables.

```
data = [(2,5), (1,2), (7,3)]
for each in itertools.starmap(operator.mul, data)
    print(each)

10
2
21
```

## tee()

## zip_longest()

## combinations()

## permutations()

## combinations_with_replacement()

## product()
