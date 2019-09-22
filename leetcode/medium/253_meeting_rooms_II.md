# 253. Meeting Rooms II

## Sorting Solution
- Runtime: O(Nlog(N))
- Space: O(N)
- N = Number of total start and end points in intervals

By separating the start and end points of each interval, we can then sort them based on their time, if the times are the same, we can break them depending on if its a start or end point.

This allows us to perform one pass over the sorted points to find number of occupied rooms.
If its a start point, we can increment the number of rooms and vice versa.

The one edge case to consider is an input like [[0,5], [5,10]] which results in 1 room needed.
Notice that the start and end points overlap between the two intervals. This means when sorting the points, we should give precedence for end points over start points during tiebreakers. 

```
from collections import namedtuple

Point = namedtuple('Point', ['time', 'is_start'])

class Solution:
    def minMeetingRooms(self, intervals: List[List[int]]) -> int:
        points = [Point(time=x[0], is_start=1) for x in intervals] + [Point(time=x[1], is_start=0) for x in intervals]
        points.sort(key=lambda x: (x.time, x.is_start))
        n_used_rooms = max_rooms = 0
        for point in points:
            if point.is_start:
                n_used_rooms += 1
            else:
                n_used_rooms -= 1
            max_rooms = max(max_rooms, n_used_rooms)
        return max_rooms
```
