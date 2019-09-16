# 146. LRU Cache

## Solution
- Runtime: O(1)
- Space: O(C)
- C = Capacity

This is a very popular interview question.

Since both get() and put() need to be O(1), this can be achieved by using a dictionary and a queue.

Will we need some type of ordering to figure out what keys are not longer needed and we will need a dictionary to determine if the key exists.
Combining both will help us achieve O(1).

Since the get() will reorder our nodes in the queue, for example, if we get a key and the key is currently in the middle of queue.
We need a way to move it to the front of the queue.
To do that, the dictionary will be a pointer to that node in the queue. 
For the dictionary, we can use the key as the key and the value will be node in the queue.
Another thing to note is that each node in the queue will need to be a doubly linked list.
During the removal process of a node in the queue, we would need to know what the previous and next nodes are.

Put() will simply add to the front of the queue and update the dictionary.
If the capacity has been reached, we can remove what is at the tail of the queue and remove the corresponding key from the dictionary.

As an added bonus, you can use a circular linked list as your queue, although it is not required.
You can also utilize a dummy node to eliminate the four cases of removing and adding nodes to a linked list. 

If you plan to use OrderedDict or some other library, the interviewer will ask you to implement it from scratch.
So don't rely on outside libraries for your hash table and queue.

```
class LRUCache:

    def __init__(self, capacity: int):
        self.queue = CircularLinkedList()
        self.key_to_node = dict()
        self.cap = capacity
        
    def get(self, key: int) -> int:
        if key in self.key_to_node:
            node = self.key_to_node[key]
            self.queue.remove(node)
            new_node = self.queue.add(node)
            return self.key_to_node[key].val
        return -1

    def put(self, key: int, value: int) -> None:
        if key in self.key_to_node:
            node_to_del = self.key_to_node[key]
            self.queue.remove(node_to_del)
        new_node = Node(key, value)
        self.queue.add(new_node)
        self.key_to_node[key] = new_node
        if len(self.key_to_node) > self.cap:
            node_to_del = self.queue.tail
            self.queue.remove(node_to_del)
            del self.key_to_node[node_to_del.key]
        
class CircularLinkedList(object):
    def __init__(self):
        self.dummy_node = Node(0, 0)
        self.dummy_node.next = self.dummy_node
        self.dummy_node.prev = self.dummy_node
        
    def add(self, new_node):
        if new_node is None or new_node is self.dummy_node:
            return
        new_node.prev = self.dummy_node
        new_node.next = self.dummy_node.next
        self.dummy_node.next.prev = new_node
        self.dummy_node.next = new_node
        
    def remove(self, node):
        if node is None or node is self.dummy_node:
            return
        p = node.prev
        n = node.next
        p.next = n
        n.prev = p
        
    @property
    def tail(self):
        if self.dummy_node.prev is not self.dummy_node:
            return self.dummy_node.prev
        return None
        
class Node(object):
    def __init__(self, key, val):
        self.prev = None
        self.next = None
        self.val = val
        self.key = key
```

## Solution with OrderedDict
- Runtime: O(1)
- Space: O(C)
- C = Capacity

For those interested in the implementation with an OrderedDict. Here is an example.

```
from collections import OrderedDict

class LRUCache:

    def __init__(self, capacity: int):
        self.cap = capacity
        self.cache = OrderedDict()

    def get(self, key: int) -> int:
        if key in self.cache:
            val = self.cache.pop(key)
            self.cache[key] = val
            return val
        return -1

    def put(self, key: int, value: int) -> None:
        if key in self.cache:
            self.cache.pop(key)
        self.cache[key] = value
        if len(self.cache) > self.cap:
            self.cache.popitem(last=False)
```
