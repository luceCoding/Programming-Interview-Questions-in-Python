# Itertools
This section will go over a few tools in python 3's itertools library.

## Iterables vs. Iterators

Before we begin, you can't understand itertools unless you understand iterators and iterables.

### Iterables

### Iterators
Iterators is an object which can iterate over an iterable. 
You can create an iterator with the **iter()** method. 
You can iterate by implementing the **__next__()** method, this returns the next item in the object.

When you use a for loop, you are actually using an iterator. An iterator is created for you and loop across your iterable.
When an iterator reaches the end of the list, it will automatically throw a StopIteration exception.
The for loop catches that exception and exits gracefully.

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
