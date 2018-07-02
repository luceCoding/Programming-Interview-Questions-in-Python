# EXPLAINATION
One of the first solutions one can think of is to have a list which you would append each new number into then sort it. After sorting it, find the median. However, that is slow, a sort would take N(logN) and you would have to do this N times which amounts to N*N(logN). Not a good run-time at all.

However, we can improve this with using heaps.
This question definitely was an innovative way of using heaps that I never encountered before. 
The idea is to use a max heap to represent the left side of the median and a min heap to represent the right side of the median. 
Based on the new number being added, it will determine whether it belongs in the left or right heap. 
After each new number added, we also need to balance each heap so that when we ask for the median, we can determine it by just looking at the top of the two heaps.
If we don't do the rebalancing at all, we won't know what are the middle numbers to use.
When it comes to determining the median there can be 2 cases. One where the heap sizes are equal and one where they are not.
If they are equal, that means we can just take the top two values from each heap and figure out the median.
If they arn't equal, we take the top value from the biggest heap as our median.

With this method, we will sort the heap in log(N) time, N times, which equates to Nlog(N). During the rebalance phase, there can be an extra sort which can bring us to Nlog(N) + log(N) but in terms of big O, it will boil down to Nlog(N).

https://www.youtube.com/watch?v=VmogG01IjYc&t=480s

# SOLUTION
```
class MedianFinder(object):

    def __init__(self):
        """
        initialize your data structure here.
        """
        self.left_max_heap = list()
        self.right_min_heap = list()

    def addNum(self, num):
        """
        :type num: int
        :rtype: void
        """
        if len(self.right_min_heap) != 0 and num >= self.right_min_heap[0]:
            heapq.heappush(self.right_min_heap, float(num))
        else:
            heapq.heappush(self.left_max_heap, float(-num))
        self.rebalance_heaps()
        #print self.left_max_heap, self.right_min_heap
        
    def rebalance_heaps(self):
        if len(self.right_min_heap) - len(self.left_max_heap) >= 2:
            right_min = heapq.heappop(self.right_min_heap)
            heapq.heappush(self.left_max_heap, -right_min)
        elif len(self.left_max_heap) - len(self.right_min_heap) >= 2:
            left_max = -heapq.heappop(self.left_max_heap)
            heapq.heappush(self.right_min_heap, left_max)
    
    def findMedian(self):
        """
        :rtype: float
        """
        if len(self.right_min_heap) > len(self.left_max_heap):
            return self.right_min_heap[0]
        elif len(self.left_max_heap) > len(self.right_min_heap):
            return -self.left_max_heap[0]
        left_val = -self.left_max_heap[0]
        right_val = self.right_min_heap[0]
        median = (left_val + right_val) * 0.5
        return median
```
