# Bloom Filters

## Use Case
Given a word, figure out if it already exists or not.

On a system design level, a hash table can work. 
However, if you have a lot of words, say a billion+ words, you start running into performance issues.
You cannot store this in memory and so there will be some overhead with disk input output and storage.
You could try to optimize as much as you can, like sharding the data into buckets with sub-hash tables but this doesn't 100% solve the latency issue.

This is where bloom filters come in, is it a popular usage for databases.
If you imagine an API like check(word) and it returns True or False.
However, the API is probabilistic, if it gives you a False it is 100% accurate, if it returns True its 90% accurate, more or less, depends.
The difference is that bloom filter uses a lot less memory than the hash table method.

## How it works
1. Starting with a bit array of a set size, say 00000000 of 8 bits.
2. Given a word, "cat", we will run this past multiple hash functions, each hash function outputs an index.
For example, two hash functions hash1('cat') and hash2('cat') gives us two indexes 2 and 5.
We will then set the bits to 00100100 in respect to its indexes.
3. Then given another word, "dog", we will run it past the hash functions as well, giving us indexes 7 and 2.
Again, setting the bit array accordingly to 00100101.
4. If we wanted to check if the word "bird" exists, we would run it past the hash functions, for example it would return indexes 5 and 1.
Since index 1 isn't set, we know "bird" does not exist.
5. Simiarly if we tried another word, like "lion" and the hash functions returned 2 and 7, the API would believe that the word "lion" exists but we never saved it.

This is why bloom filters will always accurately return if something doesn't exist but fail to 100% predict if a word does exist.
To increase the likelihood that it is correct, bloom filters will use many hash functions, this is to increase the chances to find more indexes containing zeros.

Lastly, since the bloom filters use a bit array, we can store the bit array as a string, each character containing 8 or 16 or 32 bits dependings on your operating system.
Which results in something like A90bhl158, this can represent all the set bits in a condensed manner.

## Limitations
Bloom filters require a rough estimate of how many unique elements would be stored as it would require the bit array to be determined beforehand.
Once the bit array is set, it will be hard to change it.
Simiarly, once we add an element into the bit, it will forever be added and can never be removed.
However, there is something called an invertible bloom filter, which can be used to determined which bits to remove.
I won't be discussing this topic here as it shouldn't be needed for interviews.
