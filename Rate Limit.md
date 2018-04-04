# QUESTION
Whenever you expose a web service / api endpoint, you need to implement a rate limiter to prevent abuse of the service (DOS attacks).

Implement a RateLimiter Class with an isAllow method. Every request comes in with a unique clientID, deny a request if that client has made more than 100 requests in the past second.

# EXPLAINATION

You need to use a hash table lookup for each clientID. Then each clientID will have a queue of 100 max size. The queue will hold timestamps.

Say you have a clientID with a list of 100 elements already in it. Check the last added element's timestamp with the current time which will be at the end of queue. If they are less than or equal to one second from each other then you know you are about to add the 101th request within one second apart, so return false. Remember to pop and push even if you are returning false. Therefore, everything is O(1) run-time with O(N*100) space, N being number of clientIDs.

An even more optimized solution is to use a pooling technique. For each clientID, has a pool of 100 elements. Instead of a queue, use a circular queue. That way you don't waste time creating and deleting objects. When you add a new timestamp, just move the head pointer back one and modify. When you want to check the timestamp, just check the one behind the head pointer.

The solution must use a timescale of milliseconds or better. If you were to use seconds, it wouldn't be exact enough. The seconds will be rounded up or down and you would lose precision. Even a tenth of a second would not be enough. If you had 100 requests at 0.1 of a second, then later you get another request at 1.1 of a second. How would you know that it was exactly 1.1 or 1.099 or 1.101?? 1.101 is passed one second.

# SOLUTION
```
import time
from time import sleep

class PreciseRateLimiter(object):
    def __init__(self, max_requests, time_interval_ms):
        self._max_requests = max_requests
        self._time_interval_ms = time_interval_ms
        self._clientIDs = {}
        
    def is_allowed(self, clientID):
        current_ms_time = int(round(time.time() * 1000))
        if clientID in self._clientIDs:
            if len(self._clientIDs[clientID]) >= self._max_requests:
                time_diff = current_ms_time - self._clientIDs[clientID][-1]
                self._clientIDs[clientID].pop()
                self._clientIDs[clientID].append(current_ms_time)
                if time_diff < self._time_interval_ms:
                    return False
            else:
                self._clientIDs[clientID].append(current_ms_time)
        else:
            self._clientIDs[clientID] = list()
            self._clientIDs[clientID].append(current_ms_time)
        return True
    
myLimiter = PreciseRateLimiter(100, 1000)
max_counter = 101
counter = 1
while counter <= max_counter:
    sleep(0.002)
    print 'counter: {} result: {}'.format(counter, myLimiter.is_allowed(1))
    counter += 1
```
