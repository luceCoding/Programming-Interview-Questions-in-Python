# SOLUTION (Run: O(1), Memory: O(N), N = number of keys)
This question is a very popular question, I was asked this by Amazon.

For get(), its pretty obvious that a brute force solution of having a list and searching it will get you O(n) runtime solution. 
If you use a sorted array, you get O(logN) solution, using binary search.
But if you used a hash table, you can get O(1).

However, since its a least freq used, you will need another data structure to figure out the order of your inserts and gets. 
You may immediately think of a queue, however, a queue lacks the ability to remove from anywhere in the queue.
So that leaves us with linked lists.

So we can use a hash table to point to the nodes of our linked list and use our linked list to store of the values and order of our gets and inserts.
Knowing the least used node at any given point. The further to the end of the linked list a node is, the least used it is.

With linked lists, now you have to ask yourself, is a singly linked list, doubly linked list, or circular linked list acceptable?
You can certainly do the question either or but each implementation has a different edge case and some are harder than others.
For example, how do you get the previous linked list in a singly linked list?
You can instead save the nodes in your hash table to point to the node before the target node. But it gets hairy.

Instead we can use a doubly linked list. But if you ever implemented a linked list before, there are four edge cases to consider.
- Pop from empty list
- Add to middle of list
- Add to tail of list
- Add to empty list

You can certainly trip yourself during an interview reminding to put if statements to avoid these problems. 
So I recommend using a circular linked list with a dummy node that never gets deleted. This will elimnate all these edge cases for you and the only edge case you have to remember is to not delete the dummy node.

Many interviewers will not accept your answer if you use an OrderedDict, this question is testing your linked lists abilities.

```
class LRUCache(object):

    def __init__(self, capacity):
        """
        :type capacity: int
        """
        self.cap = capacity
        self.linked_list = CircularLinkedList()
        self.key_to_node = dict()
        
    def get(self, key):
        """
        :type key: int
        :rtype: int
        """
        if key in self.key_to_node:
            target_node = self.key_to_node[key]
            self.linked_list.remove(target_node)
            self.linked_list.add(target_node) # Move to front
            return target_node.val
        return -1

    def put(self, key, value):
        """
        :type key: int
        :type value: int
        :rtype: None
        """
        if key in self.key_to_node:
            target_node = self.key_to_node[key]
            target_node.val = value
            self.linked_list.remove(target_node)
            self.linked_list.add(target_node) # Move to front
        else:
            new_node = Node(key=key, val=value)
            self.linked_list.add(new_node)
            self.key_to_node[key] = new_node
            if len(self.linked_list) != 0 and len(self.linked_list) > self.cap:
                tail_node = self.linked_list.pop()
                del self.key_to_node[tail_node.key]
                del tail_node
        
        
class CircularLinkedList():
    
    def __init__(self):
        self._dummy_head = Node(key=-1, val=-1)
        self._dummy_head.next = self._dummy_head
        self._dummy_head.prev = self._dummy_head
        self._len = 0
        
    def __len__(self):
        return self._len
        
    def add(self, new_node):
        next_node = self._dummy_head.next
        new_node.prev = self._dummy_head
        new_node.next = next_node
        self._dummy_head.next = new_node
        next_node.prev = new_node
        self._len += 1
        
    def pop(self):
        tail_node = self._dummy_head.prev
        if tail_node is not self._dummy_head:
            self.remove(tail_node)
            return tail_node
        return None
        
    def remove(self, node_to_remove):
        prev_node = node_to_remove.prev
        next_node = node_to_remove.next
        prev_node.next = next_node
        next_node.prev = prev_node
        self._len -= 1
        
        
class Node(object):
    
    def __init__(self, key, val):
        self._val = val
        self._key = key
        self.next = None
        self.prev = None
        
    @property
    def val(self):
        return self._val
    
    @val.setter
    def val(self, value):
        self._val = value
    
    @property
    def key(self):
        return self._key
```
