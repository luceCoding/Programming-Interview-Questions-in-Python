# Load Balancing
Load balancers are dedicated servers or machines made to reroute traffic or messages across a cluster of machines.
It is generally used in many large systems for this purpose.

The network of systems can be very complex, having different sub clusters of machines to allow redundancy and avoid any single point of failures.
Load balancers are used to interweave between these clusters to distribute load evenly.
So it is very common to have multiple load balancers, at least one per cluster of machines.
If one machine is getting many tasks sent to it, it is up to the load balancer to elevate that problem. 

Generally, when a request comes in, the load balancer hashes a code, that can be a userID or messageID which will route the request to a machine that the load balancer is responsible for.

## Techniques to load balancing

### Random Selection
Generally the most unpopular and least efficient of the approaches.

#### Pros
- Simple to implement on a small cluster of not that important services.

#### Cons
- Chance of getting uneven load between the servers depending on the sequence of random selection.

### Round Robin

#### Pros

#### Cons

### LFU or LRU

### Pros

### Cons

### Modding approach
When a request is sent, generally a number, usually the number of servers available, the request will be hashed aganist the mod.
The result of the mod is the server number the request will be sent to.

#### Pros
- Fairly common among smaller services with low-medium traffic.

#### Cons
- Cannot scale well if number of servers change, it will create uneven load during this period of time. Caches for several servers will become useless due to the change and requests will need to recomputed. If you were to remove a server, then your algothrim may shift the requests one across, making each server which was dedicated to a specific user, suddenly dealing with different set of users.

## Consistent Hashing
You can imagine a circle, the circle represents the possible hashes when you hash the request key.
For example, a possible of 100 hashes and 3 servers. you can have hash 0 to 33 to server 1, 34 to 66 to server 2, and 67 to 100 to server 3.
When a request comes in, if the hash lands on any of these ranges, you can determine which server to send to.

When you add a new server to the group, making a total 4, for example, the circle changes to 0 to 33 to server 1, 34 to 66 to server 2, 67 to 80 to server 3 and 81 to 100 to server 4. Notice that server 1 and server 2 become unchange. The only ones affected are server 3 and server 4, which will require their caches to be redone. Similar affect if you were to remove a server.

You may notice that this approach creates uneven load, to elevate this problem, you can rehash multiple times to distribute or create more ranges on the ring. Therefore, reducing the range gap between servers when adding or removing servers.

During a removal or addition of a server, it is generally done one at a time. For example, adding a new server then waiting a duration say an hour to allow the cache to get to a reasonable state before adding another server.
